## Switch-case

```js
switch (true) {
  case window.innerWidth > 1400:
    window.scrollTo(0, 1000);
    break;
  case 1400 > window.innerWidth && window.innerWidth > 1024:
    window.scrollTo(0, 700);
    break;
  case 1024 > window.innerWidth && window.innerWidth > 768:
    window.scrollTo(0, 400);
    break;
  default:
    window.scrollTo(0, 150);
    break;
}
```

- It is slow because the engine has to compare the value twice for each case.

```js
switch (true) {
  case window.innerWidth > 1400:
    window.scrollTo(0, 1000);
    break;
  case window.innerWidth > 1024:
    window.scrollTo(0, 700);
    break;
  case window.innerWidth > 768:
    window.scrollTo(0, 400);
    break;
  default:
    window.scrollTo(0, 150);
    break;
}
```

[Compared conditional methods](https://stackoverflow.com/questions/6665997/switch-statement-for-greater-than-less-than)

```js
if (window.innerWidth > 1400) {
  window.scrollTo(0, 1000);
} else if (window.innerWidth > 1024) {
  window.scrollTo(0, 700);
} else if (window.innerWidth > 768) {
  window.scrollTo(0, 400);
} else {
  window.scrollTo(0, 1000);
}
```

---

## Truthy & Falsy

### Truthy values in JS

- `"hello"`
- `42`
- `true`
- `[ ], [ 1, "2", 3 ] (arrays)`
- `{ }, { a: 42 } (objects)`
- `function foo() { .. } (functions)`

### Falsy values in JS

- `"" (empty string)`
- `0, -0, NaN (invalid number)`
- `null, undefined`
- `false`
- Any value that's not on this "falsy" list is "truthy."

```js
if (0) {
  () => {};
} // won't triggered

if ([]) {
  () => {};
} // // will triggered
```

---

## ? operator

### 1- ternary operator ` ? :`

```js
const isBlack = false;

const text = isBlack ? "Yes, black!" : "No, something else.";
console.log(text);
// "No, something else."
```

```js
const isBlack2 = false;

let tex2t;
if (isBlack) {
  text = "Yes, black!";
} else {
  text = "No, something else.";
}

console.log(text);
// "No, something else."
```

### 2- optional chaining --> ?.

- read the value of a property located deep

```js
const person = {
  name: "Robin Wieruch",
  pet: {
    petname: "Trixi",
  },
};

const cat = person.pet?.petname;
console.log(cat);
// "Trixi"
```

```js
const person2 = {
  name: "Robin Wieruch",
};

const cat2 = person2.pet.petname;
console.log(cat2);
// Uncaught TypeError: Cannot read properties of undefined (reading 'petname')
```

- when attempting to call a method which may not exist

```js
let result = someInterface.customMethod?.();

String.replace();
Array.map();
Object.keys();
```

```js
const str = "Hello World";

str.map(() => {}); //str.map is not a function
str.map?.(() => {}); //undefined
```

```js
characters.length > 0 && characters?.map((item, index) => <></>);
```

```js
const characters2 = [];
characters2.map((item, index) => <></>);
```

---

## `key={index}`

```js
const todoItems = todos.map((todo, index) => (
  // Only do this if items have no stable IDs
  <li key={index}>{todo.text}</li>
));
```

- We don’t recommend using indexes for keys if the order of items may change.
- This can negatively impact performance and may cause issues with component state.
- If you choose not to assign an explicit key to list items then React will default to using indexes as keys.

[example](https://jsbin.com/wohima/edit?output)

[example - classbased component](https://codepen.io/pen?editors=0010)

- the list and items are static–they are not computed and do not change;
- the items in the list have no ids;
- the list is never reordered or filtered.

---

## Controlled and uncontrolled inputs

### Controlled Component

- A Controlled Component is one that takes its current value through props and notifies
- changes through callbacks like onChange. A parent component "controls" it by handling
- the callback and managing its own state and passing the new values as props to the controlled component.
- \*\* Form element's data is handled by the React component.

### Uncontrolled Component

- A Uncontrolled Component is one that stores its own state internally,
- and you query the DOM using a ref to find its current value when you need it.
- This is a bit more like traditional HTML.
- \*\* Form element's data is handled by the DOM.

```html
<input type="text" value="{value}" onChange="{handleChange}" />
<input type="text" defaultValue="foo" ref="{inputRef}" />
<!-- Use `inputRef.current.value` to read the current value of <input> -->
```

---

## HOOKS - useState

```javascript
function useState(initialValue) {
  var value = initialValue;

  function state() {
    return value;
  }

  function setState(newVal) {
    value = newVal;
  }
  return [state, setState];
}

var [foo, setFoo] = useState(0); // using array destructuring

console.log(foo()); // logs 0 - the initialValue we gave
setFoo(1); // sets _val inside useState's scope
console.log(foo()); // logs 1 - new initialValue, despite exact same call
```

```js
// React Hooks
const [count, setCount] = useState(0);
```

- What does calling useState do?
- It declares a “state variable”. Normally, variables “disappear” when the function exits but state variables are preserved by React.

```js
function useState() {
  return;
}
```

- You might be wondering: why is useState not named createState instead?
- “Create” wouldn’t be quite accurate because the state is only created the first time our component renders. During the next renders, useState gives us the current state. Otherwise it wouldn’t be “state” at all!

---

## HOOKS - useEffect

```js
useEffect(() => {
  // side effects ...
}, [deps]);
```

### Side Effects

- A "side effect" is anything that affects something outside the scope of the function being executed.

These can be;

- Setting the page title imperatively
- manipulating DOM
- Working with timers like setInterval or setTimeout
- Measuring the width, height, or position of elements in the DOM
- Logging messages to the console or other service
- Setting or getting values in local storage
- Fetching data or subscribing and unsubscribing to services

- Network request, which has your code communicating with a third party (and thus making the request, causing logs to be recorded, caches to be saved or updated, all sorts of effects that are outside the function. )

- Pushing a new item onto an array that was passed in as an argument is a side effect

- Changing the value of a closure-scoped variable is a side effect.

- \*\*Functions that execute without side effects are called "pure" functions: they take in arguments, and they return values. Nothing else happens upon executing the function.

## Lifecycles

- Component did mount
- Component did update
- Component will unmount

![React Life Cycles](/lifecycle.png).

1- Call for all re-renders

```js
useEffect(() => {
  console.log("rendered");
});
```

2- Component did mount

invoke a side-effect once after component mounting:

```js
useEffect(() => {
  document.title = "Greetings page";
}, []);
```

3- Component did update & Component did mount

invoke a side-effect after component mounting and when dependencies changing:

```js
const [state, setState] = useState();

useEffect(() => {
  console.log("invoked only if state or props had been changed.");
}, [prop, state]);
```

![Useeffect schema](/useeffect-1.png).

4- Side-effect cleanup (Component will unmount)

```js
useEffect(() => {
  // add event listener

  return function cleanup() {
    // remove event listener
  };
}, [dependencies]);
```

Cleanup works the following way:

- After initial rendering, `useEffect()` invokes the callback having the side-effect. cleanup function is not invoked.
- On later renderings, before invoking the next side-effect callback, `useEffect()` invokes the cleanup function from the previous side-effect execution (to clean up everything after the previous side-effect), then runs the current side-effect.
- Finally, after unmounting the component, useEffect() invokes the cleanup function from the latest side-effect.

```js
const [count, setCount] = useState();

useEffect(() => {
  //do something

  return () => {};
}, [count]);
```

![Useeffect schema](/useeffect-2.png).
