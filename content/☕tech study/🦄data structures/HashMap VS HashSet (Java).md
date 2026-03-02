- [[Java Implementation of Hash Table]]

| Category                    | **HashSet**                                                                         | **HashMap**                                                      |
| :-------------------------- | :---------------------------------------------------------------------------------- | :--------------------------------------------------------------- |
| **Definition**              | A collection class that stores **unique elements**.                                 | A collection class that stores **key-value pairs**.              |
| **Data Storage**            | Stores **values (elements)** only.                                                  | Stores **key-value pairs**.                                      |
| **Duplicates**              | Does **not allow duplicate elements**.                                              | **Keys cannot be duplicated**, but **values can be duplicated**. |
| **Null Values**             | Allows **only one null value**.                                                     | Allows **one null key** and **multiple null values**.            |
| **Class Hierarchy**         | `Collection` → `Set` → `HashSet`                                                    | `Map` → `HashMap`                                                |
| **Synchronization**         | **Not synchronized** (not thread-safe).                                             | **Not synchronized** (not thread-safe).                          |
| **Performance**             | **Fast on average (O(1))** for basic operations.                                    | **Fast on average (O(1))** for basic operations.                 |
| **Internal Implementation** | Internally uses a **HashMap** (stores values as keys with a dummy object as value). | Uses its **own hash table structure** internally.                |
| **Key Methods**             | `add()`, `remove()`, `contains()`, etc.                                             | `put()`, `get()`, `remove()`, `containsKey()`, etc.              |
| **Primary Use**             | Used to store a **collection of unique elements**.                                  | Used for **efficient lookup/storage of values by key**.          |
| **Examples**                | Member IDs, unique tag lists, etc.                                                  | Mapping structures like product ID → price, user ID → age.       |

Korean VER.

|기준|**HashSet**|**HashMap**|
|---|---|---|
|**정의**|고유한 요소를 저장하는 컬렉션 클래스|키-값 쌍을 저장하는 컬렉션 클래스|
|**데이터 저장 방식**|값(value)만 저장|키(key)와 값(value)을 쌍으로 저장|
|**중복 허용**|중복 요소를 허용하지 않음|키는 중복 허용되지 않지만, 값은 중복 가능|
|**Null 허용**|단 하나의 null 값 허용|하나의 null 키와 여러 개의 null 값 허용|
|**클래스 계층**|Collection → Set → HashSet|Map → HashMap|
|**동기화**|비동기화됨|비동기화됨|
|**성능**|평균적으로 빠름 (O(1))|평균적으로 빠름 (O(1))|
|**내부 구현**|내부적으로 HashMap을 사용 (값은 더미 객체로 저장)|자체적으로 해시 테이블 구조 사용|
|**주요 메서드**|`add()`, `remove()`, `contains()` 등|`put()`, `get()`, `remove()`, `containsKey()` 등|
|**사용 목적**|고유한 요소들의 집합을 저장할 때 사용|키로 값을 효율적으로 조회/저장할 때 사용|
|**예시**|회원 ID, 유일한 태그 목록 등|상품ID → 가격, 사용자ID → 나이 등 매핑 구조|
