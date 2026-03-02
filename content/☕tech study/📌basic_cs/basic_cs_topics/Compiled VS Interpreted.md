
|                               | Compiler                                                          | Interpreter                                                       |
| ----------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------- |
| Development Convenience		<br> | You need to recompile the code every time you make a change. 👎   | You can modify and run the code immediately. 👍                   |
| Execution Speed               | Fast. 👍                                                          | Slow. 👎                                                          |
| Security                      | The program code is not exposed to users. 👍                      | The program code can be exposed. 👎                               |
| File Size                     | The entire executable file must be transferred, so it's large. 👎 | Only the source code needs to be transferred, so it's smaller. 👍 |
| Programming Languages         | Low level => C/C++                                                | High level => Python, Ruby                                        |
- Low-level languages are **usually compiled**, but not always.
- High-level languages are **usually interpreted**, but many can be compiled too.

# korean explanation
- 컴파일러
	- 프로그래밍 언어 -> 머신코드
	- 프로그램 전체를 한번에 번역한 후 완성된 머신코드로 만들어줌
	- 이미 번역 완성된 머신코드를 받는 사람의 컴퓨터로 전달
	- 단점
		- 사람들은 머신코드를 이해 못함
		- 언어로 쓰고 컴파일러가 다 될때까지 기다려야함
		- 코드를 다시 쓰면 컴파일을 매번 다시 해야함
- 인터프리터
	- 한줄씩 즉흥적으로 실행하주는 프로그램
		- 한줄씩 즉시 실행함
	- 코드를 고치고 나서 컴파일 등의 과정 없이도 바로 결과 확인 가능
	- 번역된 머신코드를 건내주지 않고 코드 자체를 보내줌
		- 받는 사람 컴퓨터에 인터프리터가 설치되어야 함