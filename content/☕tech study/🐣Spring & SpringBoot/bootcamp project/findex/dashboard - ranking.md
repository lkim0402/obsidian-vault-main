# Spec
- [[Findex project - 초급 프로젝트 1]]
```
지수 성과 분석 랭킹
- 전일/전주/전월 대비 성과 랭킹성과는 종가를 기준으로 비교합니다.

Index Performance Analysis Ranking
- Performance ranking compared to the previous day/week/month.
- Performance is compared based on the Closing Price.
```
# Problem
> `2 * n + 1` queries
- An inefficient way to do it
	- Fetch all `IndexInfo` all at once
	- Loop through all `IndexInfo`. For each `IndexInfo` : (*1 query, a loop that runs `N` times*)
		- fetch most recent `IndexData` - *1 query*
		- fetch past `x` day's `IndexData` - *1 query* 
		- calculate the performance + store the performance in a map (indexinfo : performance)
	- Get the map & sort them manually according to the performance of the `IndexData`'s closing prices 
- `2 * n + 1` problem
	- `1` query to get all `N` `IndexInfo` objects.
	- A loop that runs `N` times. Inside the loop, you make `2` separate database calls (`findRecentIndexData` and `findPastIndexData`).
	- Total queries =` 1 + (N * 2) = 2N + 1`. 
	- this doesn't scale that well then you have 100s, or 1000s of `IndexInfo`
# Solution
- To calculate the performance for any index, you need
	- most recent closing price
	- closing price from a past date
- 3 constant queries total!
## Steps
> Takes constant number of queries => `3` queries!
- Fetch list of ALL `IndexInfo` objects (`allIndexInfos`) - *1 query*
    - `List<IndexInfoDto> allIndexInfos = ...`
- **Fetch All "Current" Data**: Fetch the most recent `IndexData` in a list for all `IndexInfo` - *1 query*
	- `List<IndexData> findAllRecentIndexData(); `
	- Convert to a map: `Map<Long, IndexData> recentDataMap = ...`
		- key = indexinfo id
- **Fetch All "Past" Data**: Fetch all `IndexData` for  the past `x` days (depends on `periodType`) - *1 query*
	- `List<IndexData> findAllPastIndexData()`
	- Convert to a map: `Map<Long, IndexData> pastDataMap = ...`
		- key = indexinfo id
- **Calculate Performance:** 
	- Make ranked list `List<IndexPerformanceDto> performanceDtoList`
	- Loop through `allIndexInfos`. For each one:
		- Get the current and past `indexData` from your maps.
		- calculate the fluctuation rate
		- make a `PerformanceDto`
		- add to ranked list
- **Sorting**
	- sort `performanceDtoList` by `fluctuationRate` in descending order
	- Slice it (using `limit`) & add user given `IndexInfo` id's `RankedIndexPerformanceDto`
	- return 

## Code (1차)
```java
  public List<RankedIndexPerformanceDto> getPerformanceRank(long indexInfoId, PeriodType periodType, int limit) {

	// IndexInfo의 가장 최신 IndexData fetch
    IndexData indexData = findRecentIndexData(indexInfoId)
        .orElseThrow(() -> new NoSuchElementException("IndexData not found"));


    LocalDate currentDate = indexData.getBaseDate();
    LocalDate pastDate =
        switch (periodType) {
          case DAILY -> currentDate.minusDays(1);
          case WEEKLY -> currentDate.minusDays(7);
          case MONTHLY -> currentDate.minusMonths(1);
        };

    // 모든 IndexInfo 조회 (1 query)
    List<IndexInfo> allIndexInfos = indexInfoRepository.findAll();

    // 가장 최근 모든 IndexData 리스트 조회 (1 query)
    // map 구현: key - long IndexInfoId, value - IndexData indexData
    List<IndexData> recentIndexDataList = dashboardRepository.findAllRecentIndexData();
    Map<Long, IndexData> recentDataMap = recentIndexDataList.stream()
        .collect(Collectors.toMap(i -> i.getIndexInfo().getId(), i -> i));

    // 특정 날짜 이후 가장 최근 모든 IndexData 리스트 조회 (1 query)
    // map 구현: key - long IndexInfoId, value - IndexData indexData
    List<IndexData> pastIndexDataList = dashboardRepository.findAllPastIndexData(pastDate);
    Map<Long, IndexData> pastDataMap = pastIndexDataList.stream()
        .collect(Collectors.toMap(i -> i.getIndexInfo().getId(), i -> i));

    // 1) indexInfo당 recentIndexData, pastIndexData를 구함
    // 2) fluctuationRate 계산
    // 3) performanceDto 생성
    List<PerformanceDto> performanceDtoList = new ArrayList<>();
    allIndexInfos
        .forEach(
            i -> {
              IndexData recentIndexData = recentDataMap.get(i.getId());
              IndexData pastIndexData = pastDataMap.get(i.getId());

              // NPE 피하기
              if (recentIndexData == null || pastIndexData == null) {
                return;
              }

              // 증가
              double recentClosingPrice = recentIndexData.getClosingPrice();
              double pastClosingPrice = pastIndexData.getClosingPrice();

              // 계산
              double versus = recentClosingPrice - pastClosingPrice;
              double fluctuationRate = versus / pastClosingPrice * 100;

              PerformanceDto performanceDto =  new PerformanceDto(
                  i.getId(),
                  i.getIndexClassification(),
                  i.getIndexName(),
                  versus,
                  fluctuationRate,
                  recentClosingPrice,
                  pastClosingPrice
              );

              performanceDtoList.add(performanceDto);

            }
        );

    // PerformanceDto 리스트를 fluctuationRate로 sort하기
    List<PerformanceDto> sortedPerformanceDtoList = performanceDtoList.stream()
        .sorted(Comparator.comparing(PerformanceDto::fluctuationRate).reversed())
        .toList();

    // Sorted된 PerformanceDto를 랭킹과 합친 RankedIndexPerformanceDto로 만들기
    // Ranking : PerformanceDto
    List<RankedIndexPerformanceDto> allRankedDtos = IntStream.range(0, sortedPerformanceDtoList.size())
        .mapToObj(i -> new RankedIndexPerformanceDto(
            sortedPerformanceDtoList.get(i), // PerformanceDto
            i + 1  // ranking
      )
    ).toList();

    // limit에 맞춰서 allRankedDtos 자르기
    int trueLimit = Math.min(limit, sortedPerformanceDtoList.size());
    List<RankedIndexPerformanceDto> finalResultList = new ArrayList<>(
        allRankedDtos.subList(0, trueLimit));

    // 유저가 보낸 indexInfoId가 리스트에 있는지 확인
    boolean isUserIndexInFinalList = finalResultList.stream()
        .anyMatch(dto -> dto.performance().indexInfoId() == indexInfoId);

    // 리스트에 없으면 추가하기
    if (!isUserIndexInFinalList) {
      allRankedDtos.stream()
          .filter(dto -> dto.performance().indexInfoId() == indexInfoId)
          .findFirst() // Optional<RankedIndexPerformanceDto>
          .ifPresent(finalResultList::add); // 존재하면 finalResultList에 추가
    }

    return finalResultList;
  }
}
```

## Database calls
#### Getting all of the most recent `IndexData`
```java
/**
* 모든 IndexInfo에 대해 가장 최신의 IndexData를 한번의 쿼리로 조회합니다.
* N+1 문제를 해결하기 위해 Native SQL과 ROW_NUMBER를 사용합니다
* @return 각 지수별 가장 최신 IndexData 객체의 리스트
*/
@Query(value = """
	WITH RankedData AS (
		SELECT id, index_info_id, base_date, closing_price,
			ROW_NUMBER() OVER(PARTITION BY index_info_id ORDER BY base_date DESC) as rn
		FROM index_data
	)
	SELECT id, index_info_id, base_date, closing_price
	FROM RankedData
	WHERE rn = 1
""", nativeQuery = true)
List<IndexData> findAllRecentIndexData();
```

1. **`WITH RankedData AS (...)`**: This part creates a temporary, virtual table named `RankedData`.
	-  **`ROW_NUMBER() OVER(...)`**: A window function that assigns a unique, sequential number to each row.
		- 1,2,3...
	- **`PARTITION BY index_info_id`**:  Tells the function to restart the numbering for each different `index_info_id`
		- Groups all the rows that have the **same** `index_info_id` value together
		- all records for 'KOSPI' are in one group, all records for 'S&P 500' are in another group, and so on
	- **`ORDER BY base_date DESC`**: Within each of those "piles," it orders the rows by `base_date` from newest to oldest.
	- **`as rn`**: The calculated row number is stored in a new column called `rn`. Because of the ordering, the most recent entry for each index will always get `rn = 1`.
2. **`SELECT ... FROM RankedData WHERE rn = 1`**: Finally, the query selects only the rows from our temporary table where the rank (`rn`) is 1. This effectively filters out everything except the single most recent entry for each unique index.
#### Example

| id  | index_info_id | base_date  |
| --- | ------------- | ---------- |
| 1   | A             | 2025-08-01 |
| 2   | B             | 2025-08-01 |
| 3   | A             | 2025-07-31 |
| 4   | A             | 2025-07-30 |
| 5   | B             | 2025-07-31 |
`index_data`

| id  | index_info_id | base_date  |
| --- | ------------- | ---------- |
| 1   | A             | 2025-08-01 |
| 3   | A             | 2025-07-31 |
| 4   | A             | 2025-07-30 |
`PARTITION BY index_info_id ORDER BY base_date DESC`: 
- `PARTITION BY index_info_id`: Groups all the rows that have the **same** `index_info_id` value together
- `ORDER BY base_date DESC`: Orders in descending order (so most recent one first)    

| id  | index_info_id | base_date  | rn    |
| --- | ------------- | ---------- | ----- |
| 1   | A             | 2025-08-01 | **1** |
| 3   | A             | 2025-07-31 | **2** |
| 4   | A             | 2025-07-30 | **3** |
`ROW_NUMBER() OVER(PARTITION BY index_info_id ORDER BY base_date DESC) as rn`
- Assigns numbers, starting from 1
- Repeats for the other tables with different `index_info_id`

```java
SELECT
	id, index_info_id, base_date, closing_price
FROM
	RankedData
WHERE
	rn = 1
```
- **Final Selection**: The query then looks at this combined, ranked result and pulls only the rows `WHERE rn = 1`.
- We don't have to get the other fields because they aren't necessary for our case rn
	- the other fields will be initialized
		- object types - `null`
		- primitive types - `0`
		- boolean - `false`
#### Getting all `IndexData` in comparison to given dates
```java
/**
* 모든 IndexInfo에 대해 특정 과거 시점 / 그 이전에 가장 최신인 IndexData를 한번의 쿼리로 조회합니다. 주말이나 공휴일 데이터를 처리하며, N+1문제를
* 해결하기 위해 Native SQL과 ROW_NUMBER를 사용합니다.
*
* @param pastDate 조회 기준이 되는 과거 날짜. 이 날짜 혹은 그 이전의 데이터를 찾습니다.
* @return 각 지수별로, 기준 날짜 혹은 그 이전의 가장 최신 IndexData 객체의 리스트
*/
@Query(
value = """
  WITH RankedData AS (
	  SELECT id, indexInfoId, base_date, closing_price
		  ROW NUMBER() OVER(PARTITION BY indexInfoId ORDER BY base_date DESC) as rn
	  FROM IndexData
	  WHERE base_date <= :pastDate
  )
  SELECT id, indexInfoId, base_date, closing_price
  FROM RankedData
  WHERE rn = 1
""",
  nativeQuery = true)
List<IndexData> findAllPastIndexData(@Param("pastDate") LocalDate pastDate);
```
- same with above except the date part
- `WHERE base_date <= :pastDate`

| id  | index_info_id | base_date  | rn    |
| --- | ------------- | ---------- | ----- |
| 1   | A             | 2025-08-01 | **1** |
| 3   | A             | 2025-07-31 | **2** |
| 4   | A             | 2025-07-30 | **3** |

if `pastDate` was 2025-07-31, then:
- numbering of `rn` also starts again

| id  | index_info_id | base_date  | rn  |
| --- | ------------- | ---------- | --- |
| 3   | A             | 2025-07-31 | 1   |
| 4   | A             | 2025-07-30 | 2   |
