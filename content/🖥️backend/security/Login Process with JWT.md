![](https://calm-individual-12a.notion.site/image/attachment%3Ae544a36d-05f1-4a92-92a0-1fae17897e20%3A%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3_(35).svg?table=block&id=287c6b70-9828-81fb-94db-d054a39d0af6&spaceId=8d720266-d651-4530-b3e3-47e27aa42b6c&userId=&cache=v2)

1. 클라이언트가 서버 측에 로그인 인증 요청(Username/Password를 서버 측에 전송)
2. 로그인 인증을 담당하는 Security Filter(`JwtAuthenticationFilter`)가 클라이언트의 로그인 인증 정보 수신
3. Security Filter가 수신한 로그인 인증 정보를 AuthenticationManager에게 전달해 인증 처리를 위임
4. AuthenticationManager가 **Custom UserDetailsService**(`MemberDetailsService`)에게 사용자의 UserDetails 조회를 위임
5. **Custom UserDetailsService**(`MemberDetailsService`)가 사용자의 크리덴셜을 DB에서 조회한 후, AuthenticationManager에게 사용자의 **UserDetails**를 전달
6. AuthenticationManager가 **로그인 인증 정보와 UserDetails의 정보를 비교해 인증 처리**
7. JWT 생성 후, 클라이언트의 응답으로 전달

1번부터 7번 과정 중에서 우리는 `JwtAuthenticationFilter` 구현(2번 ~ 3번, 7번), `MemberDetailsService`(5번)을 구현합니다.