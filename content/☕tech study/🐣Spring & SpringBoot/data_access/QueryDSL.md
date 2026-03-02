
>[!Overview]
>A framework that allows you to construct type-safe queries in Java
>- It generates metamodel classes (Q-classes) from your entities, enabling compile-time checking of queries instead of runtime errors with string-based queries.

- DSL (Domain-Specific-Language)
	- A computer language specialized to a particular application domain
	- Ex) SQL, CSS, Regex, etc
- Resources
	- https://youtu.be/CtvMe7xP0gY?si=CwgISiEltbE_Eslc
# The advantages
- Type safety
	- At compile time, it tracks the classes annotated with `@Entity` and makes a `QClass` for each (contains their meta data)
	- The types for the fields
		- Integer -> `NumberPath`
		- String -> `StringPath`
		- The `*Path` extends the `*Expression` abstract class
			- The `*Expression` abstract class implements `Expression` interface
				- This abstract class contains specific methods you can use to calculate, like `NumberExpression` abstract class contains `max()`, `min()`, `random()`, etc
			- Ex) `NumberPath` extends the `NumberExpression` abstract class, which implements `Expression` interface
- Can reuse repeated conditionals
	- Entity conditionals are expressed by the `Expression` interface, and the JPQL creation and execution are by the `JPAQueryFactory`
		- They are separated, unlike traditional methods
	- So you can make different conditional methods `A`, `B`, `C`, and use them however u want to make a query!!
- Clean static queries (깔끔한 동적 쿼리)
```sql
where (
	BooleanExpression1,
	BooleanExpression2,
	BooleanExpression3
)
```
- If any `BooleanExpression` is null, it is just *ignored* like it didn't exist!!

# Quick Example
```java
BooleanExpression isSameMemberId(Long memberId) {
	if (memberId == null) return null;
	return reservation.member.id.eq(memberId); // checking if it's equal with eq
}

BooleanExpression isSameThemeId(Long themeId) {
	if (themeId == null) return null;
	return reservation.reservationSlot.theme.eq(themeId);
}

// ============= query result!!
return jpaQueryFactory.selectFrom(reservation)
	.join(reservation.member, member)
	.fetchJoin()
	.where(
		isSameMemberId(memberId), // 에약자
		isSameThemeId(themeId), // 테마
		greaterThanEqualDate(startDate), // 시작날짜
		lessThanEqualDate(endDate) // 종료날짜
	)
	.fetch();
```

# Using with `JPARepository`
- 
