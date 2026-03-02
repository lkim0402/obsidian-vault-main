- simple notes for now..

# `SecurityConfig`
- Literally like the **master rulebook** or the central configuration for your entire security setup
- The `@EnableWebSecurity` annotation is like a big switch that turns on all the security features
- you define a series of instructions (as `@Bean` methods) that tell Spring Security exactly how to behave. The most important set of instructions is the `SecurityFilterChain`
	- `.csrf(...)`: **Cross-Site Request Forgery** protection. This prevents attackers from tricking your users into submitting malicious requests without their knowledge. You've configured it to work nicely with a modern frontend (like React or Vue) by storing the protection token in a cookie.
	- `.formLogin(...)`: This sets up how users log in with a username and password.
	    - `loginProcessingUrl("/api/auth/login")`: Tells Spring Security, "Hey, when a POST request comes to this specific URL, treat it as a login attempt."
	    - `successHandler(...)` and `failureHandler(...)`: You're telling it to use your custom classes to decide what to do after a successful or failed login (e.g., return a specific JSON response).
	- `.authorizeHttpRequests(...)`: This is the **most critical part**. It's where you define your access rules, like a bouncer with a list.
	    - `requestMatchers(...).permitAll()`: You're listing all the URLs that **anyone can access** without logging in (e.g., the homepage, login page, static files).
	    - `anyRequest().authenticated()`: This is a catch-all rule that says, "For any other URL not listed above, the user **must be logged in**."
	- `.exceptionHandling(...)`: This defines what happens when security rules are broken.
	    - `authenticationEntryPoint(...)`: Handles **Authentication** failures (HTTP 401 Unauthorized). This is when a user who isn't logged in tries to access a protected page. Your custom handler decides what error message to send back.
	    - `accessDeniedHandler(...)`: Handles **Authorization** failures (HTTP 403 Forbidden). This is when a logged-in user tries to access something they don't have the permission for (e.g., a regular user trying to access an admin page).
	- `.sessionManagement(...)`: This controls user login sessions. You've configured it so that a user can only be logged in from **one place at a time** (`maximumSessions(1)`). If they log in on a new computer, the old session is invalidated.

# `UserDetails`
- An interface that represents a user in a format that Spring Security can understand
- An object that implements `UserDetails` must provide at least three key pieces of information:
	1. **`getUsername()`**: The user's username.
	2. **`getPassword()`**: The user's **hashed** password from the database.
	3. **`getAuthorities()`**: A collection of permissions the user has (e.g., `ROLE_USER`, `ROLE_ADMIN`).
- Making custom `UserDetails`
	- You can create a class that implements this interface to ***wrap your own `User` entity***
# `UserDetailsService`
- also an **interface** with only one method: `loadUserByUsername(String username)`
- It's main job is to **find and retrieve** a user's data.
- **How it Works**: When a user submits the login form with a username and password, Spring Security takes the username and passes it to your `UserDetailsService`'s `loadUserByUsername` method.
- Making custom `UserDetailsService`    
    1. Take the incoming `username` string.
    2. Query your `userRepository` to find the corresponding user in your database.
    3. If the user is found, you ***wrap your user entity into an object that implements `UserDetails` and return it***.
    4. If the user is not found, you throw a `UsernameNotFoundException`.