---
title: "Reason: JavaScript-flavored OCaml"
theme: mine
revealOptions:
  transition: fade
  showNotes: false
  controls: false
extraHTML: |
  <div id="footer" style="
    position: absolute;
    left: 30px;
    bottom: 30px;
    height: auto;
    width: auto;
    font-size: 2em;
    right: 30px;
    display: flex;

  " class="reveal">
    @jaredforsyth
    <div style="flex: 1"></div>
    <a href="https://jaredforsyth.com/type-systems-js-dev">jaredforsyth.com/type-systems-js-dev</a>
  </div>

---

# Reason

## JavaScript-flavored OCaml

---

## Defining terms
## What does Reason look like?
## What's the status of Reason?
## Comparison to other langs

---

### Defining terms

- OCaml
- Reason
- BuckleScript

---

## What does Reason look like?

---

```
let module Input = {
  type state = string;
  let component = ReasonReact.statefulComponent "Input";
  let make ::onSubmit _ => {
    ...component,
    initialState: fun () => "",
    render: fun {state: text, update} =>
      <input
        value=text
        onKeyDown=(update (fun evt {state: text} =>
          if (ReactEventRe.Keyboard.key evt == "Enter") {
            onSubmit text;
            ReasonReact.Update "";
          } else {
            ReasonReact.NoUpdate
          }
        ))
      />
  };
};
```

---

### Functions
```rust
let add x y => x + y;
let add3 = add 3;
let greet ::name ::greeting => {
  greeting ^ ", " ^ name
};
greet name::"PolyJS" greeting::"Hello";
```

---

### Types
```rust
type x = string;
type t;
type listOfInts = list int;
type maybeString = option string;
/* records */
type person = {name: string, age: int};
type country = {name: string, population: int};
let onlyTakesCountry {name} => "Country of " ^ name;
/* objects */
type personObj = {. name: string, age: int};
type countryObj = {. name: string, population: int};
let takesAnyNamedObj {.name} => greet ::name greeting::"Hi";
```

---

### Matching
```rust
let z = switch person {
| {name: "PolyJS"} => 1000
| {age: 10} => 100
| _ => 0
};
let maybeName = Some "name";
let name = switch maybeName {
| Some x => x
| None => "default name"
};
```

---

### Lists, Arrays
```rust
/* immutable, linked */
let z = [1,2,3];
switch z {
| [] => "no items"
| [first, ...rest] => "some items"
};
/* mutable, fixed length */
let ar = [|1,2,3|];
switch ar {
| [||] => "none"
| [|only|] => "one"
| _ => "more"
/* no spread */
};
```

---

### Mutability & Classes
```rust
let x = ref 0;
x := !x + 1;

let ar = [|1,2|];
ar.(0) = 5;

class myClass = {
  val mutable something = 0;
  pub set_something x => {
    something = x;
  };
};
let inst = new myClass;
inst#set_something 20;
```

---

### Modules
```rust
let module MyModule = {
  type t = string;
  let add x y => x + y;
};
type somerec = { name: MyModule.t };
let v = MyModule.add 20 30;
```

---

### Functors
```
module type Adder = {
  type t;
  let add: t => t => t;
};
let module MakeDoubleAdder (Adder: Adder) => {
  let addTwice x y => Adder.add (Adder.add x y) y;
};
let module DoubleIntAdder = MakeDoubleAdder {
  type t = int;
  let add x y => x + y;
};
DoubleIntAdder.addTwice 10 2; /* 14 */
```

---

### Syntax extensions
like rust's procedural macros
```rust
let name = "me";
let x = {j|Hello $name|j};

type person = {
  name: string,
  age: int
} [@@deriving yojson];

let x = [%bs.raw {|window.alert("this is from javascript")|}];
```

---

### Syntax extensions
you could implemente `guard let` from swift
```rust
let doSomething maybeName => {
  [%guard let Some x = maybeName][@else 10];
  /* do other things */
  5
};
/* -- becomes --> */
let doSomething maybeName => {
  switch maybeName {
  | Some x => {
    /* do other things */
    5
  }
  | _ => 10
};
```

---

## What's the status of Reason?

---

## Type system
20 years old & quite powerful

---

## Syntax
in flux

[see this PR](https://github.com/facebook/reason/pull/1299)

---

## Build tools
getting there

- bucklescript's `bsb`
- jbuilder

---

## Package manager

- npm w/ bucklescript
- opam w/ native/ocaml dev

---

## Editor integration
very cool, still a bit unstable

---

## Documentation
😕 quite poor

---

## Standard library
not yet standard

---

## Community libraries
very early days

----

## Comparison to Flow

- flow doesn't change js runtime semantics
- easier adoption (just strip the annotations)
- still have some of the sadness of js
- no real variant/sum types
- type system less mature, less sound
- flow adapts to js idioms

---

## Comparison to Elm

- elm keeps js at arm's length
- interop more cumbersome, but safer
- less mature type system
- only targets JavaScript



















