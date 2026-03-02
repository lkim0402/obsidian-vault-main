- prereq: vs code, node (at least 18)
	- extensions: ESLint, Prettier

### 2 main options
1. `create-react-app`
	- complete starter kit for react applications
	- all convenient tools preconfigured (ESLint, Prettier, etc) specifically for React
	- Problem: was developed many years ago, so uses outdated tech under the hood
	- X used for real world projects, but good way for courses/tutorials/experiments & small world projects
	- command line interface
	- `npx create-react-app@5 pizza-menu`
	- Above command gave errors:
		- ``npm config set legacy-peer-deps true``
		- `npx create-react-app pizza-menu`
		- `npm install --save-dev ajv@^7 ` ([stack overflow](https://stackoverflow.com/questions/70020046/quasar-error-cannot-find-module-ajv-dist-compile-codegen))
 
2. Vite
	- real world application
	- modern build tool that contains a template for setting up React applications
	- need to manually implement dev tools (ESLint, etc)
	- **extremely fast to reload** => good hot module replacement (HMR) and bundling!
		- automatically refresh the page when code changes
	- `npm create vite@latest`

React frameworks
- built on top of the React library, which makes it easier to build applications
- "vanilla" react apps without react frameworks aren't always necessary
- Next.js
	- routing, data fetching, server side rendering (things that react doesn't really provide out of the box)
- Remix
