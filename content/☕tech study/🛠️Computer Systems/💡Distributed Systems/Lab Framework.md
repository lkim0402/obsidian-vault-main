# Node (Abstract class)
- Abstract class that you will subclass - most fundamental class you will use
- Basic unit of computation (aka one machine)
- Notable methods:
	- `set(Timer timer, int timerLengthMillis)` -> [[Lab Framework#Timer|Timer]]
	- `send(Message message, Address to)` -> [[Lab Framework#Message|Message]]
	- message and timer handlers (naming is important!)
		- Message handler (`handleXXX(XXX message)`)
			- `handleXXX` -> XXX is name of message (e.g. `handlePongReply`) 
			- Automatically triggered when Node receives XXX message
			- Example: If you define a message class `class PingRequest extends Message`, you must create a method `public void handlePingRequest(PingRequest message)` in your `Node`. The framework will find and run this method when a `PingRequest` arrives.
		- Timer Handler (`onXXX(XXX timer)`)
			- `onXXX` -> XXX is name of timer (e.g. `onPingTimer`) 
			- Automatically triggered when a Timer you previously set() (named XXX) "fires" (its time is up)
			- Example: If you `set(new MyTimer(), 50)`, you must create a method `public void onMyTimer(MyTimer timer)`. This method will run 50ms after you set the timer.
		- This is called reflection ([Java reflection](https://www.oracle.com/technical-resources/articles/java/javareflection.html)) 
## Client node
- Interface that client Nodes should implement ([[Abstraction - Classes & Interfaces|classes & interfaces in Java (abstraction)]])
	- `sendCommand(), hasResult()`
	- `getResult()`
- Called by testing framework 
	- Starts the interaction
- Use [[Lab Framework#Synchronize|synchronized]] on all Client methods
## Server node
- No interface
- A Node to *host* the `Application`
- Receive `Message`s (likely containing **Command**s), passes them to `Application`'s `execute` method, then sends the **Result** back
- Will generally call `execute()` on an [[Lab Framework#Application|Application]]
## Application
- Where the client wants its RPC (command) to get executed -> the *actual logic you want to execute*
	- Logic for doing application-specific tasks
	- Could be anything! Key-Value store, shopping cart, game, bank account information… 
	- [[🦄Data Structures|Data structures]] to store data
- 📌Processes **Commands** and produces **Results** 
	- Each application will define its own set of Commands and Results
		- KVStore example: GETS/PUTS/APPENDS
- Interface
	- `Result execute(Command c)`
# Idempotency and Determinism
- Handlers should be deterministic and idempotent whenever possible
- **Deterministic**
	- A process *always gives the same result* when given the same input
	- Your handler's entire outcome (any state it changes, any messages it sends, any timers it sets) *must be only a function of its current state and the incoming message/timer*
	- This means no timestamps, no random numbers, no UUIDs
- **Idempotent** 
	- Receiving and handling the same unique message multiple times should have the exact same effect as handling it only once
	- When a unique message comes, the actions (e.g. changing state or creating timers) taken by it are only applied once and won’t be applied again for duplicate messages 
	- **Why:** In a real network, messages can be duplicated. Your system must not break or get into a weird state (like creating multiple new timers for a single duplicated request) when this happens.
# Data Objects
## Message
- A simple data dontainer
	- Encapsulates data passed between [[Lab Framework#Node (Abstract class)|Nodes]], it's what you `send` between Nodes
		- `send(Message message, Address to)` -> [[Lab Framework#Node (Abstract class)|notable methods of a node]]
	- Messages can contain anything (like metadata, **Commands**, or **Results**)
- Messages have no methods, but extends Serializable
- Messages are serialized/deserialized for you
	- Objects are serialized (copied into a packet) when sent and deserialized (copied out of a packet) when received
	- [[Java Serialization (and persistence)]]
## Timer
- A simple data container that you `set()` to make your own Node do something, it triggers your `onXXX` handler
	- `set(Timer timer, int timerLengthMillis)` -> [[Lab Framework#Node (Abstract class)|notable methods of a node]]
	- Triggers the timer handler when the timer fires - If you have a timer called `PingTimer`, `handlePingTimer` will be called after the timer has been set and the set duration has passed. 
- Will not be automatically reset! Need to set these manually in node. 
- Can contain anything, similar to Messages.
# Concurrency
Your `Node` will be interacting with multiple threads at once (e.g., one thread handling a user `sendCommand()` request while another thread handles an incoming network message).
## Synchronize ([[☕Java|Java keyword]])
- Acts as a lock *on the entire object*
	- Only one thread can execute a synchronized method on an object at a time
	- All other threads executing synchronized methods on that object will block until the first thread releases the lock
	- Used to prevent data from being corrupted by simultaneous modifications
- TL;DR: Use `synchronized` on all [[Lab Framework#Client node|Client]] methods
- You will see many methods that are synchronized, ex: 
	- `public synchronized Result getResult();`
	- We use method synchronization, so the entire object gets locked when a synchronized method gets called, but wait() releases the lock. 
## Wait & Notify
- are methods used _inside_ a `synchronized` block for thread coordination
- `wait()`: This thread **releases the lock** and goes to sleep (pauses).
- `notify()`: This (another) thread **wakes up one** of the waiting threads.
- **The Pattern:** You _always_ use `wait()` in a `while` loop that checks a condition.
    - `while (!condition) { wait(); }`
- Re-evaluate condition when notified - notify as a “hint”
- Ex: `PingClient.getResult()` waits `while (pong == null)`, where pong is set by `handlePongReply`!
	- Client then re-acquires the lock
# Lombok
- It's really just boilerplate
- `@EqualsAndHashCode`
	- Generates `equals` and `hashCode` methods for you
- `@Data`
	- "A shortcut for `@ToString`, `@EqualsAndHashCode`, `@Getter` on all fields, `@Setter` on all non-final fields, and `@RequiredArgsConstructor`!” - https://projectlombok.org/features/Data 
	- **All messages and timers should have `@Data`,** will lead to an explosion in state space if you don’t and you may fail some tests
		- Very common bug in the labs