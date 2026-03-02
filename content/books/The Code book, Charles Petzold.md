
> [!Note]- Progress of the book
> - [ ] 1-6 - basics (binary codes, telegraph)
> - [ ] 7-8 - counting
> - [ ] 9
> - [ ] 10
> - [ ] 11
> - [ ] 12
> - [ ] 13
> - [ ] 14
> - [ ] 15
> - [ ] 16
> - [ ] 17
> - [ ] 18
> - [ ] 19
> - [ ] 20
> - [ ] 21
> - [ ] 22
> - [ ] 23
> - [ ] 24
> - [ ] 25

# Ch 1-3
- The meaning of code in this book
	- A system for transferring information among people and machines
	- Code lets you communicate
		- English language, speech, text, sign language, etc
- Various codes are also used in computers to store and communicate numbers, sounds, music, pictures, movies, etc
- Example: Morse Code
	- In the same way that Morse code reduces written language to dots and dashes, the spoken version of the code reduces speech to *just two vowels*
		- Key: The key word here is two. Two types of blinks, two vowel sounds, two different anything, really, can with suitable combinations convey all types of information. 
	- Morse code is *binary* because the components of the code only consists of 2 things (dot and a dash)
	- Combinations
		- $\text{number of codes } = 2^{\text{number of dots and dashes}}$
		- 2 codes with only 1 dot/dash, 4 codes with 2 dots/dash, 8 codes with 3 dots/dash, so on and on
		- This is field of combinatorics (312 flashbangs)
- Braille system
	- To this day remains an invaluable system as the only way to read for blind and deaf ppl
# Ch 4
- You can make your own no-frills flashlight by disposing everything except the batteries + lightbulb, along with some short pieces of insulated wire 
- An electric circuit:
```
  ------ < L >
 |           |
 [ B ]       //
 [ B ]       |
 |           |
  -----------
```
- Components
	- **`[ B ]`**: Represents a battery (the two stacked squares)
		- `[+ - ]` where `+` is on top
	- **`< L >`**: Represents the light bulb
	- **`---`**: Represents wires
	- **`//`**: Represents the break in the circuit (the open switch or wire)
		- If these loose ends touch, the light will turn on
- The circuit is a **circle**!
	- The circular nature suggests that something is moving around the circuit -> *electricity*
- The circuit takes electrons away from the negative end of the battery and delivers them to the positive end of the battery. 
	- The reactions in the battery proceed until all the chemicals are exhausted, at which time you throw away the battery or recharge it.
	- The electrons needs to flow through wires
## Notes about Electricity and chemistry
- Electricity - electron theory
	- Atoms have neutrons, protons, and electrons
	- Neutrons and protons are bound into a nucleus and the electrons spin around
		- protons are held together by the *strong force*
		- 3 electrons, 3 protons, 4 neutrons - Lithium, one of 112 known *elements*, all has an atomic number from 1 to 112
		- atomic number -> # of protons = usually # of electrons
	- Atoms combine with other atoms to form *molecules*
		- $H_2O$
		- *compound* - 2 or more different kinds of elements
			- $O_2$ is not a compound because they're the same $O$
		- *mixture* - two or more substances (elements or compounds) that are physically mixed but not chemically bonded
	- electricity -> electrons being dislodged from atoms
	- charge
		- protons (`+`), electrons (`-`) -> means they are opposite
		- stable when they exist in equal numbers
- Notes on the static electricity -> spark of electricity
	- **The Build-Up (Friction):** When you walk on the carpet, your rubber shoes **scrape extra electrons _off_ the carpet.** These electrons are tiny negative charges.
	- **You become charged:** These extra electrons don't just stay on your shoes. They spread all over your body, like a crowd filling up a stadium. You now have _too many_ electrons, making your whole body **negatively charged**. These electrons are "unhappy" and desperately want to get away from each other and find a more balanced place.
	- **Exiting through the doorknob:** You reach for a metal doorknob. A metal doorknob is "grounded," which means it's a _massive_, open "empty stadium" for electrons. It can take on any extras you have without a problem.
	- **The spark (jump):** The _instant_ your finger gets close enough, all those crowded, unhappy electrons on your body see the "exit" and **jump at once** from your finger to the doorknob.
- In all batteries chemical reactions take place
	- *anode* (neg terminal) - reactions generate spare electrons here 
	- *cathode* (pos terminal)
## Conductors and resistors
- Why wires? and not like, air?
- *Conductors*
	- An element that has only 1 electron on the outermost shell (valence electrons). These valence electrons are loosely held by the atom's nucleus, and since they aren't held tightly they are free to move from one atom to the next
		- this free-flowing movement of electrons is the electric current
	- copper, silver, gold
- *Resistors*
	- some substances are more resistant to the passage of electricity than others
	- outermost shell are full (or almost full), the electrons in this full outer shell are held very tightly by the nucleus
	- insulator -> if a substance has a very high resistance (it doesn't conduct electricity much), like rubber/plastic
	- resistance = the tendency of a substance to impede the flow of electrons
		- measured in `ohms`
- Copper
	- The thicker the wire, lower resistance (thickness makes more electrons to move)
## Voltage and current
- *Voltage* ($E$, `volt`)
	- refers to a *potential* for doing work, exists whether or not smth is hooked up to a battery
	- If a brick is higher up than on the floor, it has more potential
- *Current* ($I$, `amp`)
	- Number of electrons actually zipping around the circuit
	- measured in *amperes* or `amps`
- Voltage is similar to water pressure, and current is similar to the amount of water flowing through a pipe
	- The amount of water flowing through a pipe (current) is directly proportional to the water pressure (voltage) and inversely proportional to the skinniness of the pipe (resistance)
		- The more water pressure, the more water that flows through the pipe
- ***Formulas***
	- In electricity, you can calculate how much current is flowing through a circuit if you know voltage and resistance
		- $I = E/R$ (Ohm's law)
			- $I$ = current in (amp)
			- $E$ = voltage, or electromotive force (volts)
			- $R$ = resistance (ohms)
		- Lightbulb - If a wire has low resistance, it can get hot and start to glow
	- Watt = a measurement of power
		- $P = E \times I$
			- $P$ = power (watt)
			- $E$ = voltage (volts)
			- $I$ = current (amp)
	- $R = E/I$
## Switches!
- Either a switch is *on/closed* or *off/open*
	- on/closed - allows electricity to flow
	- off/open - doesn't allow electricity to flow
- Either current flows or doesn't, lightbulb lights up or doesn't
- just like binary codes, simple electrical circuits are binary!

# Ch 5
## Bidirectional telegraph system
![[Pasted image 20251109142958.png]]

![[Pasted image 20251109143011.png]]
- The negative terminals of the two batteries are now connected
	- the connection is called a *common*
	- in circuit the common extends from the point where 'the point where leftmost lightbulb and battery are connected' + 'the point where the rightmost lightbulb and battery are connected'
- this reduces your wire requirements by 25%
- The two circular circuits (battery to switch to bulb to battery) still operate independently, even though they're now joined like Siamese twins.
## Using ground
- Once u establish a common, u can replace the wire... you can use the *earth* as a conductor!! 
- The *earth*
	- massive conductor of electricity + a source of and a repository for electrons. 
	- the earth is to electrons as an ocean is to drops of water
	- you can't just stick something small, you need something big to maintain substantial contact
		- Ex) a copper pole at least 8 ft long and 1/2 inch in diameter, bury the pole into the ground & connect wire to it
- *ground* = physical connection with the earth
	- sometimes "point of zero potential" -> no voltage is present
## problem 1: ground need high voltage batteries
- But can't use the ground with low-voltage batteries (like the 1.5V or 3V flashlight batteries)
	- Earth has its own resistance, 3-volt batteries doesn't have enough "push" (voltage) to overcome the earth's natural resistance
		- Remember  $I = E/R$ (Ohm's law)?
			- $I$ = current (`amp`), $E$ = voltage (`volts`)
	- The *ground* only works if u use high voltage (100V) -> A high-voltage source is strong enough to push the current through the Earth's resistance
- so the ground issue is not about distance, its about power
## problem 2 (separate): longer wires
- Using any wire over long distances is a separate problem
	- the *ground* is only a replacement for the **common** wire
	- You can't use the earth to replace all the wires because electricity must flow in a complete loop (a circuit) => ground system can never reduce the wires to 0
- the wire itself has resistance
	- **the longer wire, the more resistance, which reduces the current**
1. **One solution is to increase voltage & use lightbulbs with a much higher resistance**
	- By using a bulb with a very high resistance, you make the wire's resistance **seem small in comparison**. The wire's resistance no longer dominates, so it has much less effect
	- *The bulb's resistance* -> the good. called the "load", the job that you're trying to do: the bulb's filament resists electricity on purpose to get hot and glow
	- *The wire's resistance* -> the bad. It's what you're fighting against
	- detailed example is below
2. **Another solution is using a thicker wire**
	- but can be expensive
	- The thickness of wire is measured in American Wire Gauge, or AWG. The smaller the AWG number, the thicker the wire and also the less resistance it has.
### Example: why distance is bad
- A lightbulb needs a specific amount of **current** to get hot and glow.
1. Scenario 1: Short Wire (Your original flashlight)
	- **Voltage:** $3 \text{ Volts}$ (from your batteries)
	- **Resistance:** $4 \text{ Ohms}$ (this is the bulb's own resistance)
	- **Calculation:** $Current = 3 \text{ Volts} / 4 \text{ Ohms} = 0.75 \text{ Amps}$
	- **Result:** $0.75 \text{ Amps}$ is the perfect amount of current to make the bulb light up brightly.
2. Scenario 2: Long Wire (1-mile long)
	- Here, you have to add the resistance of the bulb plus the new resistance from the long wire
	- **Voltage:** $3 \text{ Volts}$ (same battery)
	- **Wire Resistance:** He calculates that a 1-mile _round trip_ of his cheap wire has a resistance of over **100 Ohms**.
	- **Total Resistance:** $\text{Wire Resistance} + \text{Bulb Resistance} = (100\text{ Ohms}) + 4 \text{ Ohms} \approx 104 \text{ Ohms}$
	- **Calculation:** $Current = 3 \text{ Volts} / 104 \text{ Ohms} \approx \text{less than } 0.03 \text{ Amps}$
	- **Result:** $0.03 \text{ Amps}$ is _way too small_! It's barely any current at all. The bulb won't even get warm, let alone light up. The wire's own resistance "choked" the circuit.
### Example: solution (resistance)
- One solution is to increase voltage & use lightbulbs with a much higher resistance
1. Scenario 1: Low-Voltage, Low-Resistance Bulb (Your Flashlight)
	- **Voltage:** 3 Volts
	- **"Good" Resistance (Bulb):** 4 Ohms
	- **"Bad" Resistance (1-mile wire):** 100 Ohms
	- **Total Resistance:** 104 Ohms
    - **Analysis:** The "bad" wire has 25 times more resistance (100 Ohms) than your "good" bulb (4 Ohms) -> The wire completely dominates the circuit. Almost all the battery's power is wasted just trying to push electrons through the wire, and almost no power is left to light the bulb. **The "bad" resistance wins.**
	    - A problem because the wire's 100 Ohms was 96% of the total resistance (100/104)
2. Scenario 2: High-Voltage, High-Resistance Bulb (The Book's Solution)
	- **Voltage:** 120 Volts
	- **"Good" Resistance (Bulb):** 144 Ohms
	- **"Bad" Resistance (1-mile wire):** 100 Ohms
	- **Total Resistance:** 244 Ohms
	- **Analysis:** The "good" bulb's resistance (144 Ohms) is _larger_ than the "bad" wire's resistance (100 Ohms). The wire is still the same, still "bad," and still has 100 Ohms of resistance. But in this new system, the bulb's resistance is high enough to "win" the battle. Most of the circuit's total resistance is now in the _bulb_, which is exactly where you want it.**The "good" resistance wins.**
		- The wire's 100 ohms is a smaller problem because it's only 41% of the total resistance (100/244)
# Ch 6
## Basic setup of the telegraph
![[telegraph.png|500]]
- This setup lets you send a binary signal
	- current on (click) & current off (clack)
	- A fast "click-clack" is a dot, and a slow "click...clack" is a dash
- *The Key*
	- Just a switch - when you press it, you complete a circuit & electricity flows
- *The sounder*
	- The receiver on the other end, which is basically the electromagnet. 
	- When the sender presses the key, the sounder's electromagnet turns on and pulls a metal bar, making a "click" sound. When the sender releases the key, the magnet turns off, and the bar springs back, making a "clack" sound
- *Electromagnet* is the foundation of the telegraph
	![[Pasted image 20251116223017.png]]
	- If you take an iron bar, wrap it with a couple 100 turns of thin wire, then run a current through the wire, the iron bar becomes a magnet => it then attracts other pieces of iron & steel
		- The coil has enough length and thinness that its electrical resistance keeps current at safe levels, so connecting it to a power supply doesn’t behave like a short circuit (a telegraph doesn't want a short circuit as it's too strong)
	- Remove the current, and the iron bar loses its magnetism
## Problem with the telegraph: Distance
- With longer distance, the wires increase in resistance
- The wire itself resists the flow of electricity, so the longer distance the weaker current.
- By the time the signal gets to the other end, it's too weak to make the electromagnet in the sounder strong enough to pull the metal bar & the signal dies
## Solution: Relay
![[relay.png]]
- Human relay (lol)
	- Pay a guy to sit in a hut every 200 miles. He listens to the weak "click-clack" from the first wire and then re-taps the _same message_ on a _new key_ connected to a _new battery_ and the _next_ 200-mile wire. This "refreshes" the signal.
- Relay 
	- Basically a machine that does what that guy did
	- How it works:
		1. A **weak** incoming current (from the first long wire) arrives at the relay.
			- This weak current is _just strong enough_ to power a small electromagnet inside the relay.
		2. That electromagnet pulls down a small metal lever -> the switch for a completely separate circuit.
		3. This separate circuit has its own **new, fresh battery** and the _next_ long piece of wire.
	- Basically the weak signal is just used to flip a switch
		![[relay2.png|500]]
		- This is why the relay is an **amplifier** (or more accurately, a signal repeater), because it uses a small current to control a much larger current