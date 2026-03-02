# Numeric separators
```js
const largeNumber = 123_456_789;
```
- use `_` to separate the number into 3 groups of 3 digits each
# Bigint
- represent integer values which are [too high](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) or [too low](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MIN_SAFE_INTEGER) to be represented by the `number` [primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)
	- useful in contexts requiring precise handling of large integers, such as cryptography, or when interacting with databases that use large integer identifiers
		- [[🗃️Database]]
- created by 
	- appending `n` to the end of an integer literal
	- using the `BigInt` constructor (mentioned in [[JS constructors & Examples]])
```js
const previouslyMaxSafeInteger = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991);
// 9007199254740991n

const hugeString = BigInt("9007199254740991");
// 9007199254740991n

const hugeHex = BigInt("0x1fffffffffffff");
// 9007199254740991n

const hugeOctal = BigInt("0o377777777777777777");
// 9007199254740991n

const hugeBin = BigInt(
  "0b11111111111111111111111111111111111111111111111111111",
);
```

- you can't mix `BigInt` and other types (like doing subtraction), you need explicit conversions

