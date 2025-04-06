
>[!Amplify]
>An application development platform and framework
>- Set of tools that makes it easier to build apps that embrace AWS 
>- has both [[== Frontend ==|frontend]] and [[== Backend ==|backend]] features

- diagram
	![[{30A5518B-3A0B-4807-B8A8-72818EAF8130}.png]]
- Simplifies the process of writing code
- Focus on product
	- Frontend Development Tools:
		- Provides libraries and UI components for popular frameworks ([[== React ==|React]], Vue, Angular, etc.)
		- Offers pre-built UI elements for common features like authentication forms, file uploaders, etc.
	- backend connection
		- Gives you ready-to-use code to connect your frontend to AWS services
	    - Handles the complex AWS configuration behind the scenes
	- provides the **frontend code Amplify SDK**
		- you can integrate into your web/mobile apps to talk to the backend (different AWS services)
		- u can use this SDK even if ur not using Amplify
- Build backend or host an application
	- Let amplify manage & create services
	- Use Amplify Studio

# Practical example
#### Without Amplify
1. Set up Cognito user pools manually in AWS console
2. Configure security settings and policies
3. Write code to connect your app to Cognito
4. Build UI components for login/signup screens
5. Handle tokens and authentication state

#### With Amplify
```
amplify add auth
amplify push
```

```js
import { Amplify } from 'aws-amplify';
import { withAuthenticator } from '@aws-amplify/ui-react';

// This adds a complete authentication system to your app
function App() {
  return <div>My Protected App Content</div>;
}

export default withAuthenticator(App);
```
- using Amplify SDK to your frontend code