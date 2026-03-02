
>[!Definition]
>**CI/CD** = **Continuous Integration** and **Continuous Delivery** / **Continuous Deployment**
>- A modern software development practice designed to automate the process of building, testing, and deploying code changes
>- Main goal: make software releases faster, more frequent, and more reliable

# CI vs. CD
- **Continuous Integration (CI)**
	- This is the practice of developers frequently merging their code changes into a central repository (like a `main` branch on GitHub). 
	- After each merge, an automated process kicks in to **build** the application and run a suite of **tests**. This ensures that new code doesn't break the existing functionality. If a test fails, the team is notified immediately so they can fix it.
    - **Goal**: Find and fix bugs quickly.
- **Continuous Delivery / Deployment (CD)**
	- This part of the pipeline takes over *after* the CI stage passes successfully.
    - **Continuous Delivery**: The tested code is automatically packaged and released to a testing or "staging" environment. The final step of *deploying to live production servers* still requires a manual approval.
    - **Continuous Deployment**: This goes one step further. If all tests and checks pass, the code is **automatically** deployed to the live production environment without any human intervention.
	    - [[AWS services]], [[Docker 🐳]]
    - **Goal**: Release new features to users safely and quickly.