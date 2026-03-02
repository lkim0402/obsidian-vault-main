# Spec
- [[Findex project - 초급 프로젝트 1]]
```
주요 지수 현황 요약
- 즐겨찾기된 지수의 성과 정보를 포함합니다.
- 성과는 종가를 기준으로 비교합니다.

Key Indices Summary
- Includes performance information for Favorited indices.
- Performance is compared based on the Closing Price.
```
# Problem
> This takes 1 + (N * 2) total database queries!
- Inefficient solution
	- Get all favorited `IndexInfo` + get the Ids (*1 query*)
	- Then per the Id (*N*), get the most recent `IndexData` and the past `IndexData` 
		- `findRecentIndexData` -> gets the top recent `indexData` based on  your `indexInfoId` (*1 query*)
			- `Optional<IndexData> current`
		- `findPastIndexData` -> gets the target date's (or the closest target date's) `indexData`  (*1 query*)
			- `Optional<IndexData>` `comparisonData`
		- Using those info, get the `PerformanceDto`

```java
  @Override  
  public List<PerformanceDto> getFavPerformanceDto(PeriodType periodType) {  
  
    List<IndexInfoDto> favoriteInfoDtos = indexInfoRepository.getFavorites();  
  
    return favoriteInfoDtos.stream()  
        .map(indexInfoDto -> {  
          return getPerformanceDto(indexInfoDto, periodType);  
          })  
        .filter(Objects::nonNull)  
        .toList();  
  }  
  
  private PerformanceDto getPerformanceDto(IndexInfoDto i, PeriodType periodType) {  
    long indexInfoId = i.id();  
  
    IndexData current =  
        findRecentIndexData(indexInfoId)  
            .orElseThrow(() -> new NoSuchElementException("IndexData does not exist"));  
    LocalDate currentDate = current.getBaseDate();  
  
    Optional<IndexData> comparisonData =  
        switch (periodType) {  
          case DAILY -> findPastIndexData(indexInfoId, currentDate.minusDays(1));  
          case WEEKLY -> findPastIndexData(indexInfoId, currentDate.minusDays(7));  
          case MONTHLY -> findPastIndexData(indexInfoId, currentDate.minusMonths(1));  
        };  
  
    if (comparisonData.isEmpty()) {  
      return null;  
    }  
  
    //    double currentPrice = current.closingPrice();  
    double currentPrice = current.getClosingPrice(); // 증가  
    double beforePrice = comparisonData.get().getClosingPrice();  
    double versus = currentPrice - beforePrice;  
    double fluctuationRate = versus / beforePrice * 100;  
  
    return new PerformanceDto(  
        indexInfoId,  
        i.indexClassification(),  
        i.indexName(),  
        versus,  
        fluctuationRate,  
        currentPrice,  
        beforePrice  
    );  
  }
```
# Solution
> 1 + N....

1. Get the list of favorited `IndexInfo`
2. Get the id list of #1 - `List<Long> indexInfoIdList`
3. Get the most recent `IndexData` based on the `indexInfoIdList` - *1 query*
	- Make this into a map with key : `IndexInfoId` and value  : `IndexData`
	- `Map<Long, IndexData> recentDataMap`
4. Calculate performance for each favorite using the data we fetched (#3, #4)

## Code (1차)
```java
// 즐겨 찾기한 Inde  @Override
public List<PerformanceDto> getFavPerformanceDto(PeriodType periodType) {

// 즐겨 찾기한 IndexData리스트 가져오기
//    List<IndexInfo> favoriteInfos = indexInfoRepository.getFavorites();
List<IndexInfo> favoriteInfos = null;

// 위의 리스트 토대로 id 리스트 생성
List<Long> indexInfoIdList = favoriteInfos.stream()
	.map(IndexInfo::getId)
	.toList();

// id 리스트에 있는 IndexInfo당 가장 최신 IndexData 가져오기
// key: indexInfoId, value: IndexData
List<IndexData> recentIndexDataList = dashboardRepository.findRecentByIndexInfoIds(indexInfoIdList);
Map<Long, IndexData> recentIndexDataMap = recentIndexDataList.stream()
	.collect(Collectors.toMap(i -> i.getIndexInfo().getId(), i -> i));

List<PerformanceDto> performanceDtoList = new ArrayList<>();
for (IndexInfo i : favoriteInfos) {
  long indexInfoId = i.getId();

  IndexData current = recentIndexDataMap.get(indexInfoId);
  LocalDate currentDate = current.getBaseDate();

  Optional<IndexData> comparisonData =
	  switch (periodType) {
		case DAILY -> findPastIndexData(indexInfoId, currentDate.minusDays(1));
		case WEEKLY -> findPastIndexData(indexInfoId, currentDate.minusDays(7));
		case MONTHLY -> findPastIndexData(indexInfoId, currentDate.minusMonths(1));
	  };

  if (comparisonData.isEmpty()) {
	continue;
  }

  double currentPrice = current.getClosingPrice(); // 증가
  double beforePrice = comparisonData.get().getClosingPrice();
  double versus = currentPrice - beforePrice;
  double fluctuationRate = 0.0;
  if (beforePrice != 0) {
	fluctuationRate = versus / beforePrice * 100;
  }

  performanceDtoList.add(new PerformanceDto(
	  indexInfoId,
	  i.getIndexClassification(),
	  i.getIndexName(),
	  versus,
	  fluctuationRate,
	  currentPrice,
	  beforePrice
	)
  );

}
return performanceDtoList;
}

===============================

private Optional<IndexData> findPastIndexData(long indexInfoId, LocalDate localDate) {  
  return dashboardRepository.findTopByIndexInfoIdAndBaseDateLessThanEqualOrderByBaseDateDesc(  
      indexInfoId, localDate);  
}
```

## Database calls
```java
public interface DashboardRepository extends JpaRepository<IndexData, Long> {  

/**  
 * 특정 날짜(targetDate) 혹은 그 이전의 가장 최신 IndexData를 조회합니다. 주말이나 공휴일처럼 특정 날짜에 IndexData가 없는 경우를 처리하는 데  
 * 핵심적인 역할을 합니다.  
 */Optional<IndexData> findTopByIndexInfoIdAndBaseDateLessThanEqualOrderByBaseDateDesc(  
    long indexInfoId, LocalDate baseDate);


  
/**  
 * 리스트에 있는 indexInfoId당 특정 indexInfoId에 해당하는 가장 최신 IndexData를 조회합니다.  
 */@Query(  
    "SELECT d FROM IndexData d WHERE d.indexInfo.id IN :indexInfoIds AND d.baseDate = " +  
    "(SELECT MAX(d2.baseDate) FROM IndexData d2 WHERE d2.indexInfo.id = d.indexInfo.id)"  
)  
List<IndexData> findRecentByIndexInfoIds(@Param("indexInfoIds") List<Long> indexInfoIds);

...
}

```

- `findTopRecentByIndexInfoIds`
	1. **`SELECT d FROM IndexData d`**
	    - This part starts by selecting the entire `IndexData` object (aliased as `d`).
	2. **`WHERE d.indexInfo.id IN :indexInfoIds`**
	    - 1st filter
	    - checks if the `indexInfo.id` is in `indexInfoIds` (the list you passed)
	3. `(SELECT MAX(d2.baseDate) FROM IndexData d2 WHERE d2.indexInfo.id = d.indexInfo.id)`
		-  For each row from the 1st query, it performs a check:
		    - `(SELECT MAX(d2.baseDate) FROM IndexData d2`: It runs a sub-query that asks, "For this specific index ID, what is the absolute latest `baseDate`?"
			    - `d2` is just a 2nd alias for `IndexData`
		    - It only keeps the original row if its `baseDate` matches that latest date.
- `findTopPastByIndexInfoIdsAndBaseDate`
