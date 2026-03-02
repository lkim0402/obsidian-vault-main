
>[!Overview]
>Focuses on verifying that the application’s features work correctly from the end user’s perspective
>- The broadest scope in the diagram

![[spring-testing.png|900]]

- Typically, functional tests are performed by the developers who built the application, but often they are handled by specialized QA teams or external QA vendors. 
	- [[Frontend]] developers may also run light functional tests to ensure the server-side application behaves properly.
- Because functional tests involve many components—such as[[APIs|API calls]], HTTP communication, and [[🗃️Database|database]] connections—they are not considered [[Unit Test|unit tests]]. (Although not shown in the diagram, these tests may also involve external services, adding further complexity.)
