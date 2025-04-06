> **This process really isn’t as hard as it seems.** You need to spend a good amount of time researching and learning, but once you have your components picked out, actually assembling everything is really easy. I know this whole thing can seem daunting, but I promise you that if I can do it, you can do it.

> Your research phase should not end when you are done building your computer. It is always a good idea to keep learning about new things and better ways to utilize your computer to get the most out of your hardware.

- Goals:
	- Dual monitor with full linux setup
	- AI applications running, machine learning projects (simulations/devlog/etc) blender, game development, art projects, coding
	- GAMES!! 
		![[Pasted image 20240821004514.png]]
- Resources
	- The ultimate beginner's guide: [beginnersguide - buildapc](https://www.reddit.com/r/buildapc/wiki/beginnersguide/)
	- PC builder's glossaey: https://artofpc.com/pc-builders-glossary/
	- [r/buildapc](https://www.reddit.com/r/buildapc/)
	- [pcpartpicker](https://pcpartpicker.com/)
	- https://www.reddit.com/r/buildapc/wiki/compatibility/parts/
- The parts
	- [[PC#CPU (Processor)|CPU]]
	- [[PC#RAM (Memory) 작업대 (공간)|RAM]]
# The Parts
## CPU (Processor)
^c021f5
![[Pasted image 20240821005450.png|300]] ^d9297f
>The brain, or the central hub.
- Two main comapnies: Intel and AMD
- Cores = number of worlers
	- Dual core = 2 workers. Enables more data to be processed at the same time.
	- Currently, most consumers can choose between quad-core, hexa-core and octa-core processors, and higher end platforms can have anywhere from 12 to 64 cores per CPU.
	- individual processing units within the CPU
	- Each core can independently execute its own thread (or process)
- Thread = arms
	- Hyperthreading = running two threads or processes simultaneously (a quad-core processor with Hyper-Threading would appear to the operating system as an eight-core processor)
- Clock speed (frequency) = brain
	- How many actions it does per second -> Computer speed up!
	- measured in Hertz, with Ghz (gigahertz) and Mhz (megahertz)
	- When comparing 2 of the same line of processors, say the i5-4470 and the i5-4570, the higher the clock speed the better
	- Overclocking = process of increasing the clock speed of a computer's CPU (or other components like the GPU and RAM) beyond the manufacturer's specified limits to achieve better performance.
- Architecture = appearance
- Processor "cache" (pronounced cash)
	- A small, high-speed memory located directly on or very close to the CPU, designed to store and quickly access frequently used data and instructions, improving the overall performance of the processor.
	- Main goal of the cache is to reduce the time the CPU needs to access data from the main memory (RAM), which is slower
	- The fastest memory available to the CPU, but limited in size
	- Usually measured in KB or MB (as opposed to GB which RAM is typically measured)
	- Important to processor!
- Cooling solutions
	- CPUs need it to avoid overheating
	- Most AMD CPUs come with a stock fan (but noisy), but most Intel CPUs don't 
	- Main options: Air cooling or water cooling
- Modern CPUs are built with the x86-64 instruction set architecture (ISA), which means they can address up to 64 bits of data. Always choose 64 bit.
- My CPU for LG Gram: 8 × Intel® Core™ i5-1035G7 CPU @ 1.20GHz
	![[Pasted image 20240824203123.png]]
	- - i 는 코어 시리즈, 7은 숫자
	- i3 - 저사양~중간정도 사양의 게임 돌리기, 가벼운 프로그램
	- i5 - 중~상옵 게임 돌리기, 일반적인 프로그램 무난하게 돌림
	- i7 - 고사양 게임 돌리기, 무거운 프로그램(설계, 영상편집 등), 다중작업(여러 프로그램 동시에 돌리기)
	- i9 - 워크스테이션, 서버용으로 주로 쓰임. 9세대부터는 체급이 약간 작아진 모델이 일반 데스크탑 제품으로 출시되기도 함
- Resources
	- https://www.reddit.com/r/buildapc/wiki/compatibility/parts/cpu/

## RAM (Memory)
![[Pasted image 20240824203621.png|300]]
- Thinking space for the brain! The more you get the more the CPU can think about at any one time. It processes more data more, so the speed goes up too. (작업대, 공간)
- 디자이너에게 중요. 많을록 굿! 디자이너라면 16GB 이상 추천
- Modern platforms, both Intel and AMD, use 4th generation Double Data Rate RAM (DDR4). DDR4 RAM is available in sticks (DIMMs) from 4GB to 128GB, for both desktops and laptops (SO-DIMMs). RAM is not compatible between generations, and they are keyed differently to avoid accidental insertion.
    - 4GB (gigabytes) is currently standard. DDR3 is standard, but some slightly older motherboards (and current ones) use DDR2. 
    - They come at different 'speeds' but this will make little difference to you.
- RAM specifications will vary depending on your remaining budget.
- Resources
	- https://www.reddit.com/r/buildapc/wiki/compatibility/parts/ram/
## GPU (Graphics card)
![[Pasted image 20240824204335.png]]
> Graphics processing unit (GPU) is a specialized electronic circuit designed to accelerate the processing of images and visual data for output to a display. Started out for video game graphics but has expanded to other areas
- An analogy
	- From a computing standpoint, the [[PC#CPU (Processor)|CPU]] generally handle programs in a “serial” fashion. For example in your grocery list the CPU will sprint down the fruits aisle, then the meats, then the dairy at a blazing 3.0ghz speed. A GPU differs from this by instead having multiple ‘people’ walk to each aisle in “parallel”. This ability to perform relatively smaller and repetitive tasks are often best on a GPU. 
	- If you think about rendering the same 3D model in a game repeatedly, the GPU will perform far greater than a CPU. Using a dedicated GPU will relieve some of the load from your CPU and allow it to perform the tasks is it better suited to.
- Types
	- Discrete GPU (dedicated graphic cards)
		- Visibly attached to the [[PC#Motherboard|motherboard]]. 
		- Typically have their own memory to work with, ensures that the graphic card has resources to do the work
		- Upgraded/removed easily as needed
		- Required for anything intensive (video gaming, video processing, parallel computing, etc)
	- Integrated GPU
		- GPUs integrated into the [[PC#Motherboard|motherboard]] generally and share the same memory as the [[PC#CPU (Processor)|CPU]]
		- Spends time fighting with the [[PC#CPU (Processor)|CPU]] to get memory from the [[PC#RAM (Memory)|RAM]], it still doesn’t guarantee it has the memory it needs to perform the work they need
		- Advantages
			- more cooler (slimmer system, good for laptops)
			- less expensive (no need to buy separate GPU)
		- Disadvantages
			- Greatly reduces peformance compared to discrete
- Companies: AMD, Intel, Nvidia
- Graphics cards are among the fastest changing component out there. Many builds were often tell you to wait until the end to purchase a graphic card if you don’t obtain all your parts in one lump group. This is because every 2-3 years AMD and Nvidia trade blows over the performance crown. It is helpful to follow these trends regarding when parts are released, but you must look at each company individually because when one releases a generation it takes awhile for the other to react.
- GPU cooling solutions
	- Needs to be cooled like the [[PC#CPU (Processor)|CPU]]. 
	- Every GPU has a heatsink attached to the GPU to cool the processor and many newer graphic cards will even sport fans or entire cooling systems that take up two expansion slots on the motherboard and case (these fans are removable)
- Connecting 2 GPUs
	- Each company has own technology and requirements for this
- The 1000s digit (generation) >>> 10s digit (performance)
- Useful for 3D programs like Blender, games
- Resources
	- https://www.reddit.com/r/buildapc/wiki/compatibility/parts/gpu_sc/#wiki_graphics_cards
## Motherboard
- 