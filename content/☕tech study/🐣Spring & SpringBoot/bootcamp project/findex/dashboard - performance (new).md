- What N+1 Problem Means = **N+1 = 1 initial query + N additional queries**
- In your old code:
	- **1 query**: Get list of favorites
	- **N queries**: One `findPastIndexData()` call for each favorite item
- So if you had 50 favorites, you'd make **51 database calls** instead of just **2**!

```java
// 1 query - getting favorites ✅
List<IndexInfoDto> favoriteInfos = indexInfoService.findAllByFavorite(true);

// 1 query - getting recent data ✅  
List<IndexData> recentIndexDataList = dashboardRepository.findRecentByIndexInfoIds(indexInfoIdList);

// N queries - THIS IS THE PROBLEM! ❌
for (IndexInfoDto i : favoriteInfos) {
    Optional<IndexData> comparisonData = switch (periodType) {
        case DAILY -> findPastIndexData(indexInfoId, currentDate.minusDays(1));    // Query #1
        case WEEKLY -> findPastIndexData(indexInfoId, currentDate.minusDays(7));   // Query #2  
        case MONTHLY -> findPastIndexData(indexInfoId, currentDate.minusMonths(1)); // Query #3...
    };
}
```
- Each `findPastIndexData()` call was likely hitting the database individually.

# Solution
```java
@Override
public List<PerformanceDto> getFavPerformanceDto(PeriodType periodType) {

// 즐겨 찾기한 IndexData리스트 가져오기
List<IndexInfoDto> favoriteInfos = indexInfoService.findAllByFavorite(true);
// 위의 리스트 토대로 id 리스트 생성
List<Long> indexInfoIdList = favoriteInfos.stream().map(IndexInfoDto::getId).toList();

LocalDate currentDate = LocalDate.now();
LocalDate pastDate = calculateMinusDate(periodType);

// 날짜에 맞춰서 가져올 수 있는 가장 가까운 날짜의 IndexData
List<IndexData> currentDataList = dashboardRepository
	.findMostRecentByIndexInfoIdsAndMaxDate(indexInfoIdList, currentDate);

List<IndexData> pastDataList = dashboardRepository
	.findClosestPastByIndexInfoIdsAndTargetDate(
		indexInfoIdList,
		pastDate,
		pastDate.minusDays(30)); // 30일 전

// 시간복잡도 O(1)을 위해 Map으로 변환
Map<Long, IndexData> currentDataMap = currentDataList.stream()
	.collect(Collectors.toMap(data -> data.getIndexInfo().getId(), Function.identity()));

Map<Long, IndexData> pastDataMap = pastDataList.stream()
	.collect(Collectors.toMap(data -> data.getIndexInfo().getId(), Function.identity()));


List<PerformanceDto> performanceDtoList = new ArrayList<>();
for (IndexInfoDto i : favoriteInfos) {
  long indexInfoId = i.getId();

  IndexData current = currentDataMap.get(indexInfoId);
  IndexData past = pastDataMap.get(indexInfoId);

  // 둘중 하나라도 없으면 스킵
  if (current == null || past == null) {
	continue;
  }

  double currentPrice = current.getClosingPrice(); // 증가
  double beforePrice = past.getClosingPrice();
  double versus = currentPrice - beforePrice;
  double fluctuationRate = 0.0;
  if (beforePrice != 0) {
	fluctuationRate = versus / beforePrice * 100;
  }

  performanceDtoList.add(
	  new PerformanceDto(
		  indexInfoId,
		  i.getIndexClassification(),
		  i.getIndexName(),
		  versus,
		  fluctuationRate,
		  currentPrice,
		  beforePrice));
}
return performanceDtoList;
}
```

- Removed the `N + 1` approach
- 

#### Database calls 

<<<< 이거 제대로 이해하고 직접 써보기
```java
  // 각 IndexInfo의 가장 최신 IndexData를 가져옴
  @Query("""
        SELECT id1 FROM IndexData id1 
        WHERE id1.indexInfo.id IN :indexInfoIds 
        AND id1.baseDate = (
            SELECT MAX(id2.baseDate) 
            FROM IndexData id2 
            WHERE id2.indexInfo.id = id1.indexInfo.id 
            AND id2.baseDate <= :maxDate
        )
        """)
  List<IndexData> findMostRecentByIndexInfoIdsAndMaxDate(
      @Param("indexInfoIds") List<Long> indexInfoIds,
      @Param("maxDate") LocalDate maxDate);

  // 각 IndexInfo의 targetDate과 가장 가까운 최신 IndexData를 가져옴
  @Query("""
        SELECT id1 FROM IndexData id1 
        WHERE id1.indexInfo.id IN :indexInfoIds 
        AND id1.baseDate = (
            SELECT MAX(id2.baseDate) 
            FROM IndexData id2 
            WHERE id2.indexInfo.id = id1.indexInfo.id 
            AND id2.baseDate <= :targetDate 
            AND id2.baseDate >= :minDate
        )
        """)
  List<IndexData> findClosestPastByIndexInfoIdsAndTargetDate(
      @Param("indexInfoIds") List<Long> indexInfoIds,
      @Param("targetDate") LocalDate targetDate,
      @Param("minDate") LocalDate minDate);
```

- `findMostRecentByIndexInfoIdsAndMaxDate`
	- For each index ID you provide, this query asks the database, "Find the newest data point you have for this index, but don't look at any data after today (`:maxDate`)
	- fetches the **single most recent** `IndexData` record for each index in a given list & on or before a specified maximum date
	- how it works
		- **Outer Query:** It starts by looking at all `IndexData` rows (`id1`) whose `indexInfo.id` is in your list (`:indexInfoIds`).
		- **Correlated Subquery 🔍:** For **each row** the outer query considers, this subquery runs. It finds the latest possible date (`MAX(baseDate)`) for that specific index (`id2.indexInfo.id = id1.indexInfo.id`) that occurs on or before the `:maxDate` you provide.
		- **Result:** The outer query only returns the `IndexData` row whose `baseDate` exactly matches the result of the subquery. This effectively filters the data down to just one record per index: the most recent one available up to your cutoff date.
- `findClosestPastByIndexInfoIdsAndTargetDate`
	- similar to above but with lower bound
	- finds the most recent `IndexData` record for each index that falls **within a specific date window**.
	- **How it works:**
		- This query works just like the first one, but the subquery has an extra condition: `AND id2.baseDate >= :minDate`.
		- This extra condition creates a search window 📅. The query will only consider data points between `:minDate` and `:targetDate` (inclusive).

#### Performance Comparison

| Scenario      | Old Code Queries | New Code Queries |
| ------------- | ---------------- | ---------------- |
| 10 favorites  | 12 queries       | 2 queries        |
| 50 favorites  | 52 queries       | 2 queries        |
| 100 favorites | 102 queries      | 2 queries        |

---
# Additional Notes
`Collectors.Map()`
```java
Map<Long, IndexData> currentDataMap = allIndexDataList.stream()
    .filter(indexData -> indexData.getBaseDate().equals(currentDate))
    .collect(Collectors.toMap(
        indexData -> indexData.getIndexInfo().getId(), // KEY function
        Function.identity()                            // VALUE function
    ));
```
- Steps
	- Takes a stream of `IndexData` objects
	- Converts them into a `Map<Long, IndexData>`
	- **KEY**: `indexData.getIndexInfo().getId()` - uses `IndexInfo` ID as the map key
	- **VALUE**: `Function.identity()` - uses the `IndexData` object itself as the value

#### Repository method
- Fetching ALL`IndexData` that is within the `List<LocalDate> baseDates`
```java
List<IndexData> findByIndexInfoIdInAndBaseDateIn(List<Long> indexInfoIds, List<LocalDate> baseDates);
```
- Spring automatically generates the SQL from the method name
	- `findBy` = SELECT query
	- `IndexInfoIdIn` = WHERE index_info_id IN (...)
	- `And` = AND operator
	- `BaseDateIn` = AND `base_date` IN (...)
- Roughly generated SQL
```sql
SELECT * FROM index_data 
WHERE index_info_id IN (100, 200, 300) 
  AND base_date IN ('2025-08-06', '2025-07-30')
```