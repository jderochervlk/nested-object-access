# Nested Object Access

TypeScript-powered util to work with nested objects using dot-separated keys.

This package exports the following stuff:

## Helper type: NestedKeys
Returns a union of all dot-separated paths to entries in a nested object.

```ts
type TestDict = {
  b: "bb";
  c: { d: "dd"; w: "ww"; e: { p: "pp" } };
  t: { l: "ww"; e: { q: "pp" } };
  i: { d: "DD"; o: "oo" };
};

// Default functionality
// "b" | "c" | "t" | "i" | "c.d" | "c.w" | "c.e.p" | "t.e.q" | "t.l" | "i.d" | "i.o" | "c.e" | "t.e"
type TestKeys = NestedKeys<TestDict>;
// Get only the paths to primitive values
// "b" | "c.d" | "c.w" | "c.e.p" | "t.e.q" | "t.l" | "i.d" | "i.o"
type TestKeysPr = NestedKeys<TestDict, "primitives">;
// Get only the paths to nested objects
// "c" | "t" | "i" | "c.e" | "t.e"
type TestKeysObj = NestedKeys<TestDict, "objects">;
// Get only up to a certain depth
// "b" | "c" | "t" | "i" | "c.d" | "c.w" | "c.e" | "t.e" | "t.l" | "i.d" | "i.o"
type TestKeysWithDepth = NestedKeys<TestDict, any, 2>;
```

## Helper type: RetrieveNested
Returns the type that is at a dot-separated path in an object.

```ts
type TestDict2 = { c: { e: { p: "pp" } } }

// { e: { p: "pp"; }; }
type TestNestedPath1 = RetrieveNested<TestDict2, "c">;
// { p: "pp"; }
type TestNestedPath2 = RetrieveNested<TestDict2, "c.e">;
// "pp"
type TestNestedPath3 = RetrieveNested<TestDict2, "c.e.p">;
```

## Function: getNested
Function version of RetrieveNested.

```ts
import { getNested } from "nested-object-access";
const testDict = { c: { e: { p: "pp" } } };
console.log(getNested("c.e.p")); // "pp"
```

## TODO
- You can currently only pass a `type Sth = {...}` into the helper functions, not `interface Sth {...}` because of missing index signature...