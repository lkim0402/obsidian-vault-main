- We will learn how to make and communicate rigorous and formal arguments
	- Make arguments - what kind of reasoning is allowed & what kind of reasoning can lead to errors?
	- Communicate arguments - using one of the common languages of computer scientists (no one is going to use your code if you can’t tell them what it does or convince them it’s functional)
# Symbolic Logic (1-8)
- Symbolic logic ->A language.. 
	- syntax: words and rules for combining words into sentences
	- semantics: ways to assign meaning to words and sentence
- lets us mechanically simplify expressions & make arguments, letting us *focus on rules of logic*
## *Proposition*
- A statement that has a truth value (it is `true` or `false`) and it is "well-formed"
- Example
	- All cats are mammals -> `true`and a proposition
	- All mammals are cats ->`false` and a proposition (well informed & has truth value)
	- `x + 2 = 5` -> NOT a proposition, does NOT have a fixed truth value!
- propositional variables: `p`, `q`, `r`, `s`...
- **Logical connectives**
	- $\land$ -> and
	- $\lor$  -> or
	- $\lnot$  -> not
	- $\rightarrow$ ->implication (if-then)
	- $\oplus$ -> exclusive or
		- exactly one of the 2 is true
	- $\leftrightarrow$ ->  biconditional
		- $(p \rightarrow q) \land (q \rightarrow p)$
		- $p$ and $q$ have the same truth value
- **Implications**
	- Ways to connect propositions. Like a *promise*.
		- If $p$ then $q$
		- $p$ implies $q$
		- whenever $p$ is true $q$ must be true
		- $q$ if $p$
		- $p$ only if $q$
	- If it is raining, then I have my umbrella. $p \rightarrow q$
	- $\lnot p \lor q \equiv p \rightarrow q$ (the columns are equal!)

| $p$ | $q$ | $p \rightarrow q$ | $\lnot p$ | $\lnot p \lor q$ |
| --- | --- | ----------------- | --------- | ---------------- |
| T   | T   | T                 | F         | T                |
| T   | F   | F                 | F         | F                |
| F   | T   | T                 | T         | T                |
| F   | F   | T                 | T         | T                |
- The last 2 lines -> *vacuous truth*
	- An implication is false exactly when you can *demonstrate I'm lying*
- $p \rightarrow q$ and $q \rightarrow p$ are different
- **Compound proposition**
	- Robbie knows the Pythagorean Theorem if he is a mathematician and took geometry, and he is a mathematician or did not take geometry.
		- $p$ -> "Robbie knows the Pythagorean Theorem"
		- $q$ -> "Robbie is a mathematician"
		- $r$ -> "Robbie took geometry"
	- $((q \land r) \rightarrow p ) \land (q \lor \lnot r)$
- **Logic order of operations**
	- Parenthesis, negation, and or/exclusive or, implication, biconditional
- **Logical equivalence**
	- Two propositions are *equal* (`=`) if they are character for character identical
	- Two propositions are *equivalent* ($\equiv$) if they have the same truth value
		- $p \land q \equiv p \land q$ and $p \land q \equiv q \land p$
		- $\equiv$ is an *assertion over all possible truth values* that $p$ and $q$ always have same truth values. It's different from $\leftrightarrow$ (which is a proposition)
## *Simplification and proofs (+table of properties)* 
- **De Morgan's Laws**
	- $\lnot (p \lor q) \equiv \lnot p \land \lnot q$ 
		- "my code compiles or there is a bug" = “my code does not compile and there is not a bug”
	- $\lnot(p \land q) \equiv \lnot p \lor \lnot q$
- **Law of Implication**
	- $\lnot p \lor q \equiv p \rightarrow q$
- **Properties of logical connectives**
	![[logical connectives.png]]
- Given 2 propositions, can we write an algorithm to determine if they are equivalent?
	- Yes -> Generate the truth tables for both propositions and check if they are the same for every entry.
## *Converse & Contrapositives*
- Implication: $p \rightarrow q$
- Contrapositive: $\lnot q \rightarrow \lnot p$
- Converse: $q \rightarrow p$
- Inverse: $\lnot p \rightarrow \lnot q$

| $p$ | $q$ | $p \rightarrow q$  (implication)📌 | $q \rightarrow p$ (converse) | $\lnot p$ | $\lnot q$ | $\lnot p \rightarrow \lnot q$ (inverse) | $\lnot q \rightarrow \lnot p$ (contrapositive) 📌 |
| --- | --- | ---------------------------------- | ---------------------------- | --------- | --------- | --------------------------------------- | ------------------------------------------------- |
| T   | T   | T                                  | T                            | F         | F         | T                                       | T                                                 |
| T   | F   | F                                  | T                            | F         | T         | T                                       | F                                                 |
| F   | T   | T                                  | F                            | T         | F         | F                                       | T                                                 |
| F   | F   | T                                  | T                            | T         | T         | T                                       | T                                                 |
- Implication and contrapositive are equal
## *Vocabs*
- A proposition is a
	- *tautology* if it is always true.
		- $p \lor \lnot p$
		- if $p$ is true or if $p$ is false, either case the statement is true
	- *contradiction* if it is always false.
		- $p \oplus p$ 
		- if $p$ is true or if $p$ is false, either case the statement is false
	- *Contingency* if it can be both true and false.
		- $(p\rightarrow q) \land p$
		- if $p$ is true and $q$ is true, statement is true
		- if $p$ is true and $q$ is false, statement is false
## *Different forms of writing logic (table)*
![[Pasted image 20260210152201.png]]

- They are just alternate notations for the same underlying ideas 
	- no new concepts, but just new representations
- In the future, you will use any/all of them.
## *Normal forms*
- main goal
	- Standard ways of translating a truth table into a proposition. 
	- We already did these in lecture when we translated implications into an expression only using ands, ors, and nots! 
	- Once you translate into one of these forms, don’t simplify your expression any further!
- (Canonical) Normal Forms (CNF)
	- AND of ORs
	- Method: 
		1. Read the FALSE rows of the truth table 
		2. OR together the negations of all the variable settings in the false row 
		3. AND together the false rows
- Disjunctive normal form (DNF)
	- OR of ANDs
	- Method: 
		1. Read the TRUE rows of the truth table 
		2. AND together all the variable settings in a given (true) row 
		3. OR together the true rows
## *Predicates, quantifiers, domain restriction, negating/nesting*
- *Predicate*
	- A function that outputs T or F
		- `Cat(x):= “x is a cat”`
		- `LessThan(x,y):= “x"`
		- The numbers and types of inputs can change, only requirement is output is a boolean
	- Examples
		- $x$ is prime or $x^2$ is odd or $x=2$
		- Prime(x) $\lor$ Odd($x^2$) $\lor$ Equals(x,2)
		- domain of discourse is numbers
- *Domain of discourse*
	- Types of inputs allowed into our predicates
- *Quantifiers*
	- $\forall(x)$ : The statement is true for every $x$
		- $\forall x (p(x) \land q(x))$ : For every $x$ in our domain, $p(x)$ and $q(x)$ both evaluate to true
	- $\exists(x)$ : There is some $x$ that works
		- $\exists x (p(x) \land q(x))$: There is n $x$ in our domain for which $p(x)$ and $q(x)$ are both true

|**Symbol**|**Command**|**Meaning**|**Example**|
|---|---|---|---|
|$\forall$|`\forall`|For all / For every|`\forall x \in S` $\rightarrow$ $\forall x \in S$|
|$\exists$|`\exists`|There exists|`\exists y > 0` $\rightarrow$ $\exists y > 0$|
|$\nexists$|`\nexists`|There does not exist|`\nexists z \in \mathbb{R}` $\rightarrow$ $\nexists z \in \mathbb{R}$|
|$\exists!$|`\exists !`|There exists exactly one|`\exists ! x` $\rightarrow$ $\exists ! x$|
- Evaluating predicate logic
	- $\forall x (Even(x) \rightarrow Equal(x,2))$
	- Is this true? it actually depends on the domain! (if domain is prime numbers, it's true)

- *Domain restriction*
	- Example 1: If the cat is fat, then it is happy. Domain of discourse - mammals.
		- $\forall x[(Cat(x) \land Fat(x)) \rightarrow Happy(x)]$
			- This is wrong because if Cat(x) is false then statement becomes vacuous truth
		- $\forall x[Cat(x) \land (Fat(x) \rightarrow Happy(x)]$
			- For all mammals, that mammal is a cat and if it is fat it is happy. It's wrong because it's saying all mammals are cats!!
		- $\forall x[Cat(x) \rightarrow ((Fat(x) \rightarrow Happy(x))]$👍
			- $x$ doesn't have to be a cat (it can be a rock), if $x$ is a cat it doesn't have to be fat, the rules didn't break and it will still return true
		- $\exists x[Cat(x) \land (Fat(x) \rightarrow Happy(x)]$👍
			- There exists a mammal where it is a cat and if cat is fat then it is happy
	- Example 2: There is a dog who is not happy. Domain of discourse - mammals.
		- $\exists x[Dog(x) \rightarrow \lnot Happy(x)]$
			- There is a mammal, such that if it is a dog then it is not happy.
			- Wrong because if i put cat s $x$ then it's  vacuous truth
		- $\exists x[(Dog(x) \land \lnot Happy(x))]$ 👍
			- There is a mammal that is both a dog and not happy
			- Correct
	- How it works
		- For $\forall$, use implication $\rightarrow$ for domain restriction
			- we need to **skip** irrelevant items (make them True) -> make vacuously true statements
			- Ex) in mammals, if u just want to check for cats, u need to ignore all other mammals (dogs, etc) and just give true for the rest
		- For $\exists$, use $\land$ for domain restriction
			- we filter out irrelevant items 
			- U need it to NOT hold true for vacuously true statements and only let it work for specific predicates
- *Negating quantifiers*
	- Steps
		- Switch the quantifier 
		- Negate the expression inside
	- Examples
		- $\exists x (Prime(x) \land Even(x))$ becomes: $\forall x (\lnot Prime(x) \lor \lnot Even(x))$
- *Nested Quantifiers*
	- $\forall x \exists y \, P(x,y)$
		- For every x there is y such that P(x,y) is true
	- $\exists x \forall y \, P(x,y)$
		- There is an x such that for all y, P(x,y) is true
	- Examples
		- Everyone is friends with someone
			- $\forall x (\exists y \, AreFriends(x,y))$ or $\forall x \exists y \, AreFriends(x,y)$
		- Someone is friends with everyone
			- $\exists x (\forall x \, AreFriends(x,y))$ or $\exists x \forall x \, AreFriends(x,y)$

## *Theorems and proofs*
### Definitions
- *Theorem*
	- A statement that has been proven to be true
- *Proof*
	- A valid argument that establishes a statement to be true
- Others
	- *claim* = the statement we're about to prove
	- *lemma* = small theorem, used to prove a bigger theorem
	- *corollary* = small theorem, proven using a bigger theorem
- Objects to work with
	- *Integer*
		- Any real number with no fractional part
	- `Even(x)`
		- An integer `x` is even if and only if there is an integer `k` such that `x = 2k`
	- `Odd(x)`
		- An integer `x` is odd if and only if there is an integer `k` such that `x = 2k + 1`
- Definition are **always** *if and only if*
- *Arbitrary variable*
	- A variable that is part of the domain of discourse & u know nothing about. every element of the domain could be plugged in and proof should still work
### Direct proof
- One strategy or proving statements of the form $\forall x [P(x) \rightarrow Q(x)]$ (universal statements)
- Template 
	![[direct proof template.png]]
- Direct proof examples
	- ![[direct proof example.png]]
	- ![[odd direct proof.png]]
	- ![[square direct proof.png]]
- Steps
	- Introduction
		- Declare an arbitrary variable for each $\forall$ quantifier
		- **Assume** the left side of the implication
	- Core of the proof
		- Unroll the predicate definitions
		- Manipulate towards the goal (using creativity, algebra, etc)
		- Reroll definitions into the right side of the implication
	- Conclude that you have proved the claim
### Inference proof + rules
- *Inference proof*
	- A step-by-step logical argument used to demonstrate that a conclusion follows necessarily from a set of premises (like a mathematical proof but for logic)
- Steps
	1. You start with **Premises** (statements assumed to be true).
	2. You apply **Rules of Inference** (valid logical moves) to these premises to create new true statements.
	3. You repeat this until you reach the **Conclusion**.
- *Modus Ponens*
	- $[(p \rightarrow q) \land p] \rightarrow q]$
		- An inference rule where -> If a rule applies, and the condition is met, the result must happen.
		- Most fundamental and common rule of inference. We use it A LOT.
	- Logic structure:
		- $p \rightarrow q$ (if $p$ is true, then $q$ is true)
		- $p$ (we know $p$ is true)
		- $\therefore q$ (therefore, $q$ must be true)
$$
\begin{array}{rl}
p \to q & (\text{Premise}) \\
p & (\text{Premise}) \\
\hline
\therefore q & (\text{Modus Ponens})
\end{array}
$$
- Example
	- We know $p \rightarrow q$ and $\lnot q$. We want to conclude $\lnot p$, how can we prove this?
	- Attempt
		1. $p\rightarrow q$ : given
		2. $\lnot q$ : given
		3. $\lnot q \rightarrow \lnot p$ : contrapositive of 1
		4. $\lnot p$: modus ponens of 3 and 2
- *Other inference rules*
$$
\begin{array}{c c}
\boxed{\text{\large Eliminate } \land} \quad \dfrac{A \land B}{\therefore A, B} & 
\boxed{\text{\large Intro } \land} \quad \dfrac{A, B}{\therefore A \land B} \\[3em]
\boxed{\text{\large Eliminate } \lor} \quad \dfrac{A \lor B, \neg A}{\therefore B} & 
\boxed{\text{\large Intro } \lor} \quad \dfrac{A}{\therefore A \lor B, B \lor A}
\end{array}
$$

### Direct Proof rule
- Remember in direct proofs
	- we use to prove implications, usually $\forall x [P(x) \rightarrow Q(x)]$ (universal statements)
	- we did this in english and made an assumption & use it to do the proof
- *Direct proof rule*
	- $A \Rightarrow B$
		- It is not a single "fact", it's an observation that we've done a proof. 
		- If I can assume A is true, I can derive B
$$
\begin{array}{c c}
 \quad \dfrac{\text{Write a proof "given A conclude B"}}{A \rightarrow B} & 
 \quad \dfrac{A \Rightarrow B}{ A \rightarrow B} \\[3em]
\end{array}
$$
$$
\begin{array}{rl}
A \Rightarrow B & \\
\hline
A \to B
\end{array}
$$
- Example
	![[direct proof example1.png]]
	- We used inference proof (writing proof in symbols) & used direct proof
	- We don't actually know if $p$ is true or not! But we're proving that $p \rightarrow r$
		- No one told me that it's raining outside today. But I'm proving that *if it's raining outside today, then I will have my umbrella*. 
### Inference Proofs in Predicate logic

$$
\begin{array}{c c}
\boxed{\text{\large Eliminate } \forall} \quad \dfrac{\forall x P(x)}{\therefore P(a) \text{ \large for any } a} & 
\boxed{\text{\large Intro } \exists} \quad \dfrac{P(c) \text{ \large for some } c}{\therefore \exists x P(x)} \\[4em]
\boxed{\text{\large Intro } \forall} \quad \dfrac{P(a); \text{ } a \text{ \large is \textbf{arbitrary}}}{\therefore \forall x P(x)} & 
\boxed{\text{\large Eliminate } \exists} \quad \dfrac{\exists x P(x)}{\therefore P(c) \text{ \large for a \textbf{fresh} } c}
\end{array}
$$
- terms
	- *arbitrary* 
	- *fresh* = means that $c$ is a new symbol (there isn't another $c$ somewhere else in our proof)
- Examples
	- ![[example1proof with quantifiers.png]]
	- ![[Pasted image 20260211111118.png]]
		- for 1.3, it's not required to have "variable is arbitrary" as a step before using it, but many ppl find it helpful
- More about fresh and arbitrary
	- Suppose we know $\exists x P(x)$. Can we conclude $\forall x P(x)$?
		- attempt?
			1. $\exists x P(x)$    Given
			2. $P(a)$          Eliminate $\exists$ (1)
			3. $\forall x P(x)$    Intro $\forall$ (2)
		- this proof is **wrong**! (take $P(x)$ as a "prime number")
		- $a$ wasn't **arbitrary**. We knew something about it, it's the $x$ that exists to make $P(x)$ true.
	- Rules
		- You can trust a variable to be arbitrary if you introduce it as such. 
			- If you eliminated a $\forall$ to create a variable, that variable is arbitrary. Otherwise it's not arbitrary, it depends on something.
		- You can trust a variable to be fresh if the variable doesn't appear anywhere else (just use a new letter.)
- Find the bug
	- Steps
		1. $\forall x \exists y \text{ Greater}(y, x)$      Given
		2. Let $a$ be an arbitrary integer      --
		3. $\exists y \text{ Greater}(y, a)$            Elim $\forall$ (1)
		4. $\text{Greater}(b, a)$                   Elim $\exists$ (3)
		5. $\forall x \text{ Greater}(b, x)$            Intro $\forall$ (4)
		6. $\exists y \forall x \text{ Greater}(y, x)$      Intro $\exists$ (5)
	- The trap is in step 4, and the bug is in step 5. You cannot turn a variable (like $a$) into '$\forall x$' if any other constant in the formula (like $b$) depends on $a$.
# Set theory/Arithmetic (10-19)

// need to start from lecture 10, 10/18
## *Set theory*
## *English proofs*
## *Sets and modular arithmetic*
## *Number theory*
## *Induction*
## *Strong induction*

# Models of computation (20-28)