# Simulated 8-bit Adder in TypeScript

Personal experiment to practice TypeScript. Playing around with overload signatures, bitwise operators and logic.

Type checks feel bloated to me - any tips to clean them up would be greatly appreciated!

Inspired by [Crash Course's ALU-video](https://youtu.be/1I5ZMmrOfnA).

---

[![picture 1](images/diagram.png)](https://youtu.be/1I5ZMmrOfnA)

<p align="center"><small><i>Diagram from Crash Course's video illustrating an 8-bit ripple carry adder</i></small></p>

---

```ts
// ---------------------
// SIMULATED LOGIC GATES
// ---------------------

type BitInt = 0 | 1;
type BitString = "0" | "1";

function xor(a: boolean, b: boolean): boolean;
function xor(a: BitInt, b: BitInt): BitInt;
function xor(a: BitString, b: BitString): BitString;
function xor(a: any, b: any): any {
  if (typeof a !== typeof b) throw "Inputs must be of same type.";
  if (typeof a === "boolean" && typeof b === "boolean") return a !== b;
  if (typeof a === "number" && typeof b === "number") return a ^ b;
  if (typeof a === "string" && typeof b === "string")
    return String(Number(a) ^ Number(b));
}

function and(a: boolean, b: boolean): boolean;
function and(a: BitInt, b: BitInt): BitInt;
function and(a: BitString, b: BitString): BitString;
function and(a: any, b: any): any {
  if (typeof a !== typeof b) throw "Inputs must be of same type.";
  if (typeof a === "boolean" && typeof b === "boolean") return a && b;
  if (typeof a === "number" && typeof b === "number") return a & b;
  if (typeof a === "string" && typeof b === "string")
    return String(Number(a) & Number(b));
}

function or(a: boolean, b: boolean): boolean;
function or(a: BitInt, b: BitInt): BitInt;
function or(a: BitString, b: BitString): BitString;
function or(a: any, b: any): any {
  if (typeof a !== typeof b) throw "Inputs must be of same type.";
  if (typeof a === "boolean" && typeof b === "boolean") return a || b;
  if (typeof a === "number" && typeof b === "number") return a | b;
  if (typeof a === "string" && typeof b === "string")
    return String(Number(a) | Number(b));
}

// ---------------------
// SIMULATED HALF ADDER
// ---------------------

function halfAdder(a: boolean, b: boolean): [boolean, boolean];
function halfAdder(a: BitInt, b: BitInt): [BitInt, BitInt];
function halfAdder(a: BitString, b: BitString): [BitString, BitString];
function halfAdder(a: any, b: any): [any, any] {
  const sum = xor(a, b); // XOR
  const carry = and(a, b); // AND
  return [carry, sum];
}

// ---------------------
// SIMULATED FULL ADDER
// ---------------------

function fullAdder(a: boolean, b: boolean, c: boolean): [boolean, boolean];
function fullAdder(a: BitInt, b: BitInt, c: BitInt): [BitInt, BitInt];
function fullAdder(
  a: BitString,
  b: BitString,
  c: BitString
): [BitString, BitString];
function fullAdder(a: any, b: any, c: any): [any, any] {
  const [c1, s1] = halfAdder(a, b);
  const [c2, s2] = halfAdder(s1, c);
  const c3 = xor(c1, c2);
  return [c3, s2];
}

// ---------------------
// SIMULATED 8-bit ADDER
// ---------------------

type ByteInt = [BitInt, BitInt, BitInt, BitInt, BitInt, BitInt, BitInt, BitInt];
type ByteString = `${BitString}${BitString}${BitString}${BitString}${BitString}${BitString}${BitString}${BitString}`;
type ByteBool = [
  boolean,
  boolean,
  boolean,
  boolean,
  boolean,
  boolean,
  boolean,
  boolean
];

function add8Bit(a: ByteInt, b: ByteInt): ByteInt;
function add8Bit(a: ByteBool, b: ByteBool): ByteBool;
function add8Bit(a: ByteString, b: ByteString): ByteString;
function add8Bit(a: any, b: any): any {
  let [c, s] = halfAdder(a[7], b[7]);

  const sum = [
    undefined,
    undefined,
    undefined,
    undefined,
    undefined,
    undefined,
    undefined,
    s,
  ];

  for (let i = 6; i >= 0; i--) {
    [c, s] = fullAdder(a[i], b[i], c);
    sum[i] = s;
  }

  if (typeof a === "string" && typeof b === "string" && typeof c == "string")
    return [sum.join(""), c];
  return [sum, c];
}
```
