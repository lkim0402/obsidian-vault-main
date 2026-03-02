Before
```java
public void update(String newUsername, String newEmail, String newPassword, UUID newProfileId) {

	boolean anyValueUpdated = false;
	
	if (newUsername != null && !newUsername.equals(this.username)) {
		// this.username = newUsername;
		setUsername(newUsername);
		anyValueUpdated = true;
	}
	
	if (newEmail != null && !newEmail.equals(this.email)) {
		// this.email = newEmail;
		setEmail(newEmail);
		anyValueUpdated = true;
	}
	
	if (newPassword != null && !newPassword.equals(this.password)) {
		// this.password = newPassword;
		setPassword(newPassword);
		anyValueUpdated = true;
	}
	
	if (newProfileId != null && !newProfileId.equals(this.profileId)) {
		// this.profileId = newProfileId;
		setProfileId(newProfileId);
		anyValueUpdated = true;
	}

	
	if (anyValueUpdated) {
		this.updateTimeStamp();
	}
}
```

Updated:
```java
public void update(String newUsername, String newEmail, String newPassword, UUID newProfileId) {  
  
    boolean anyValueUpdated = updateField(newUsername, this.username, this::setUsername) ||  
            updateField(newEmail, this.email, this::setEmail) ||  
            updateField(newPassword, this.password, this::setPassword) ||  
            updateField(newProfileId, this.profileId, this::setProfileId);  
  
    if (anyValueUpdated) {  
        this.updateTimeStamp();  
    }  
}  
  
/**  
 * 필드 값을 업데이트하고, 변경 여부를 반환하는 헬퍼 메서드  
 * @param newValue 새로운 값  
 * @param oldValue 기존 값  
 * @param setter   값을 할당할 setter 메서드 (ex: this::setUsername)  
 * @return       값이 실제로 변경되었으면 true  
 */private <T> boolean updateField(T newValue, T oldValue, Consumer<T> setter) {  
    if (newValue != null && !newValue.equals(oldValue)) {  
        setter.accept(newValue); // setter 메서드 호출  
        return true;  
    }  
    return false;  
}
```

-  `<T>`: "이 메서드는 제네릭(Generic)입니다" 라는 선언
	- **의미:** `<T>`는 "이 메서드 안에서 **타입을 위한 placeholder(자리 표시자)**를 사용할 거야. 그 placeholder의 이름은 `T`야"라고 컴파일러에게 알려주는 선언입니다.
	- The `T` in all parameters (`newValue`, `oldValue`, `setter`) is all the same `T`!
- `Consumer<T>`
	- 자바에서 제공하는 **함수형 인터페이스(Functional Interface)**
		- `T`타입의 **데이터를 하나 받아서(consume) 어떤 동작을 수행**, void return type
	- u pass method reference
		- ex) `this::setUsername`
		- `setUsername` accepts a String and returns void, which matches exactly with `Consumer<T>`
- `setter.accept(newValue)`
	- `updateField` 메서드 안에서 이 코드는 "파라미터로 받아온 그 동작(`setter`)을 `newValue`를 가지고 실행해!" 라는 뜻이 됩니다
	- `this::setUsername`이 넘어왔다면 `setUsername(newValue)`가 실행됩니다.