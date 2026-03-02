# Top-down approach (Previously)
> Start with a high-level view of the system, then break it down into smaller, more manageable subcomponents (functions)
- Disadvantages:
	- You can only fully test the system after integration, which may lead to late discovery of compatibility issues
	- If you want to modify a component, you would have to check all the other code as well
- diagram
	![[topdown.png|500]]
# Bottom-Up approach (Mostly OOP)
>Start by defining the system's individual parts first. Once the individual components are detailed, they are integrated into larger modules
- OOP
	- One of the many approaches to programming (Procedural, Imperative, Structured, Declarative, Functional, etc)
	- [[☕Java]], [[Python]] (mostly), Kotlin, Rudy, Swift, C#
	- Using objects => function + data
- diagram
	![[bottomup.png|500]]
- Reusability of low-level components, focused problem solving, and increased modularity
	- easier to test => just test individual components
- Disadvantages:
	- It's harder to visualize the full system architecture from the beginning
	- Without a top-level plan, it may be unclear what exactly each component should do
	- More oriented towards human understanding, so sometimes you'll need to sacrifice speed or storage efficiency
- Use for big projects, or projects with big data
- X recommended for programs that need to maximize efficiency (like smart watches)

