# React Interview Questions and Answers (106 Questions)

**Organization:** Fundamentals → setup and project structure → JSX → components → routing → hooks → lifecycle → forms → code splitting → advanced topics.

**Labels:** Importance labels are preserved in a simple format. Repeated questions refer back to the main explanation where appropriate. Question 72 was missing in the supplied list, so the original numbering is preserved.

---

## 1. React Basics

### Q1. What is React? What is the role of React in software development? [V. IMP.]

**Answer:**

React is an open-source JavaScript library used to build user interfaces, especially interactive, component-based web applications. It was developed at Meta.

React helps developers divide a user interface into small, reusable components. When data changes, React updates the relevant parts of the UI.

**Example:** In an e-commerce application, the header, product card, shopping cart and checkout form can each be separate React components.

**Role in software development:**
- Builds interactive user interfaces.
- Encourages reusable components.
- Manages UI state and rendering.
- Supports single-page applications.
- Integrates with routing, API clients and state-management libraries.

React is primarily a UI library; developers choose additional tools for routing, data fetching and application architecture.

### Q2. What are the key features of React? [V. IMP.]

**Answer:**

1. **Component-based architecture:** UI is divided into reusable components.
2. **Declarative UI:** Developers describe what the UI should look like for a given state.
3. **Virtual DOM and reconciliation:** React calculates UI changes and commits necessary updates to the host environment.
4. **One-way data flow:** Data commonly flows from parent components to children through props.
5. **Hooks:** Functional components can use state, effects, context and other React features.
6. **JSX:** A syntax extension that lets developers describe UI using JavaScript-like markup.
7. **Ecosystem:** Works with tools for routing, testing, forms, server rendering and state management.

### Q3. What is DOM? What is the difference between HTML and DOM?

**Answer:**

DOM stands for Document Object Model. It is a programming interface that represents an HTML document as a tree of objects (nodes). JavaScript can read and modify this tree.

- **HTML:** Markup text that describes the document structure.
- **DOM:** The browser's in-memory object representation of the parsed document.

Example HTML:

```html
<h1 id="title">Hello</h1>
```

JavaScript can access its DOM element:

```javascript
const heading = document.getElementById("title");
heading.textContent = "Welcome";
```

The browser parses HTML to construct the DOM. JavaScript can then change the DOM, and the browser updates the rendered page.

### Q4. What is Virtual DOM? What is the difference between DOM and Virtual DOM? [V. IMP.]

**Answer:**

The Virtual DOM is a conceptual in-memory representation of the UI that React uses to describe what should be rendered. When state or props change, React renders a new description, compares it with the previous one, and determines the updates to commit to the real DOM.

| Real DOM | React's UI representation (often called Virtual DOM) |
|---|---|
| Browser-managed document tree | In-memory representation of the UI |
| Directly represents document nodes | Represents the desired UI structure |
| DOM mutations can trigger browser work such as style/layout | React calculates changes before committing updates |
| Used by the browser to display the page | Used by React during rendering and reconciliation |

**Important:** Virtual DOM does not mean React always updates only one DOM node or that every React update is automatically faster. React's reconciliation helps determine updates; performance also depends on component design, rendering work and browser operations.

### Q5. What are React Components? What are the main elements of them? [V. IMP.]

**Answer:**

A React component is a reusable, independent piece of UI. It can receive input through props, maintain state when needed, and return React elements describing what should appear on the screen.

A typical functional component includes:
- A function definition.
- A component name starting with an uppercase letter.
- Optional props and hooks.
- A returned JSX/React element.

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

Components can be composed together:

```jsx
function App() {
  return (
    <main>
      <Welcome name="Arbaaz" />
      <Welcome name="Sara" />
    </main>
  );
}
```

### Q6. What is SPA (Single Page Application)? [V. IMP.]

**Answer:**

A Single Page Application (SPA) is a web application that initially loads a main HTML document and then updates the displayed content dynamically as users navigate or interact, often without requesting a completely new HTML page for every navigation.

**How it works:**
1. The browser loads the app's HTML, JavaScript and CSS.
2. JavaScript starts the application.
3. User interactions update application state.
4. React renders the UI for the new state.
5. A client-side router can change the URL and show another view without a full page reload.

**Advantages:** Smooth navigation, rich interactions and reuse of frontend state.

**Disadvantages:** Initial JavaScript load can be larger, SEO and initial rendering may need extra work, and the frontend must manage routing and API errors.

React is commonly used to build SPAs, but React applications can also use server-side rendering and other rendering strategies.

### Q7. What are the 5 advantages of React?

**Answer:**

1. **Reusable components:** Reduces duplication.
2. **Declarative programming:** UI logic is easier to reason about.
3. **Large ecosystem:** Many libraries and community resources are available.
4. **Efficient update model:** Reconciliation helps manage UI updates.
5. **Flexible integration:** React can be used in an existing application or with different backend technologies.

### Q8. What are the disadvantages of React?

**Answer:**

1. React mainly handles the UI; routing, forms and global state may require additional libraries.
2. The ecosystem has many choices, which can make architecture decisions difficult.
3. Developers need to understand JavaScript, JSX, hooks and rendering behavior.
4. Large client-side bundles can affect initial loading if not optimized.
5. SEO and initial rendering may require server-side rendering or static generation.
6. Poor state or component design can cause unnecessary renders and complex code.

### Q9. What is the role of JSX in React? [V. IMP.]

**Answer:**

JSX (JavaScript XML) is a syntax extension that lets developers write UI markup-like expressions inside JavaScript.

```jsx
const element = <h1>Hello React</h1>;
```

JSX is transformed by build tooling into JavaScript calls that create React elements. It is not HTML and browsers do not understand JSX directly.

**Benefits:** It makes component structure readable, supports JavaScript expressions, and keeps UI markup close to component logic.

### Q10. What is the difference between Declarative and Imperative Syntax? [IMP.]

**Answer:**

- **Declarative:** Describe the result you want; the framework manages the steps.
- **Imperative:** Specify the exact steps required to achieve the result.

Imperative DOM example:

```javascript
const heading = document.createElement("h1");
heading.textContent = "Hello";
document.body.appendChild(heading);
```

Declarative React example:

```jsx
function App() {
  return <h1>Hello</h1>;
}
```

In React, you describe the UI for the current state. React manages the rendering and DOM updates.

---

## 2. React Basics – Project Setup

### Q11. What is Arrow Function Expression in JSX?

**Answer:**

An arrow function is a concise JavaScript function syntax. In JSX, arrow functions are often used for event handlers, callbacks and inline component logic.

```jsx
function Button() {
  const handleClick = () => {
    console.log("Clicked");
  };

  return <button onClick={handleClick}>Click</button>;
}
```

An inline callback is also possible:

```jsx
<button onClick={() => console.log("Clicked")}>Click</button>
```

Be careful not to call the handler during rendering. `onClick={handleClick}` passes the function; `onClick={handleClick()}` invokes it immediately.

### Q12. How to set up your first React project?

**Answer:**

A common modern setup is Vite.

1. Install Node.js (which includes npm).
2. Create the project:

```bash
npm create vite@latest my-react-app -- --template react
```

3. Move into the folder and install dependencies:

```bash
cd my-react-app
npm install
```

4. Start the development server:

```bash
npm run dev
```

5. Open the local URL shown in the terminal.

Vite is a build tool and development server. Create React App was historically common but is no longer the default recommendation for new React projects.

### Q13. What are the main files in a React project?

**Answer:**

A typical Vite React project contains:

- `index.html`: HTML entry page containing the root element.
- `src/main.jsx`: JavaScript entry point that mounts the React app.
- `src/App.jsx`: Main/root application component.
- `src/index.css` or `src/App.css`: Stylesheets.
- `package.json`: Project metadata, scripts and dependencies.
- `package-lock.json`: Locked dependency versions for npm.
- `node_modules/`: Installed packages.
- `public/`: Static assets served as-is.

The exact names and layout depend on the project template and build tool.

### Q14. How does a React app load and display components in the browser? [V. IMP.]

**Answer:**

A typical client-rendered React app follows this sequence:

1. The browser requests `index.html`.
2. The HTML includes a root element, commonly `<div id="root"></div>`, and loads the JavaScript entry bundle.
3. The browser executes the JavaScript entry point.
4. `createRoot()` connects React to the root DOM element.
5. `root.render(<App />)` renders the root component.
6. React renders child components and commits the required DOM updates.
7. On state or prop changes, React renders again and reconciles the UI.

Example:

```jsx
// main.jsx
import React from "react";
import { createRoot } from "react-dom/client";
import App from "./App";

createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

`StrictMode` helps identify certain development-time issues; it does not render an extra visible UI wrapper.

### Q15. What is the difference between React and Angular? [V. IMP.]

**Answer:**

| React | Angular |
|---|---|
| UI library | Full application framework |
| Primarily JavaScript/JSX (TypeScript also common) | TypeScript-centered |
| Flexible; developers select many supporting tools | Provides an integrated framework structure |
| Uses components and hooks | Uses components, templates, services and dependency injection |
| Routing and global state commonly use separate packages | Includes official routing and dependency injection |
| UI updates use React reconciliation | Uses Angular's change-detection and rendering mechanisms |

Both can build large SPAs. The choice depends on team skills, project needs and preferred architecture.

### Q16. What are the other 5 JavaScript frameworks other than React?

**Answer:**

React is technically a UI library rather than a complete framework. Five popular JavaScript UI frameworks/libraries or frontend frameworks include:

1. **Angular** – Full-featured application framework.
2. **Vue.js** – Progressive UI framework.
3. **Svelte** – Component framework that compiles components.
4. **Ember.js** – Opinionated framework for ambitious web applications.
5. **SolidJS** – Reactive UI library/framework with fine-grained reactivity.

The category is not identical for all five; some are frameworks and some are libraries. Next.js is also popular, but it is a React framework rather than a separate alternative to React.

### Q17. Is React a Framework or Library? What is the difference? [V. IMP.]

**Answer:**

React is a **JavaScript library** focused on building user interfaces.

- A **library** provides functionality that the application calls when needed. Developers retain more control over architecture and tool selection.
- A **framework** generally provides a broader structure and conventions for building an application and often controls more of the application lifecycle.

React focuses on rendering UI. Developers commonly add routing, data fetching, form handling and state-management tools separately. Frameworks such as Angular provide more integrated application capabilities.

### Q18. How does React provide Reusability and Composition? [IMP.]

**Answer:**

React supports reusability through components and composition.

- Define a component once.
- Pass different data through props.
- Combine smaller components to create larger UI sections.
- Use `children` to allow a component to wrap or compose other content.

```jsx
function Card({ title, children }) {
  return (
    <section className="card">
      <h2>{title}</h2>
      {children}
    </section>
  );
}

function App() {
  return (
    <Card title="Profile">
      <p>User details appear here.</p>
    </Card>
  );
}
```

The same `Card` component can be reused with different titles and children.

### Q19. What are State, Stateless, Stateful and State Management terms? [V. IMP.]

**Answer:**

- **State:** Data that changes over time and affects what a component displays or how it behaves.
- **Stateful component:** A component that owns or manages state, often using hooks such as `useState` or `useReducer`.
- **Stateless component:** A component that does not own local state and derives its output from props or other inputs.
- **State management:** The approach used to create, update, share and organize application data.

Example:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

The `count` value is state. `Counter` is stateful because it manages that state.

### Q20. What are Props in JSX? [V. IMP.]

**Answer:**

Props (properties) are read-only inputs passed from a parent component to a child component.

```jsx
function User({ name, role }) {
  return <p>{name} works as {role}</p>;
}

function App() {
  return <User name="Arbaaz" role="Developer" />;
}
```

Props allow components to be reusable and configurable. A child should not directly mutate its props. To change data, the parent can pass a callback function to the child.

---

## 3. React Project – Files & Folders

### Q21. What is NPM? What is the role of node_modules folder? [V. IMP.]

**Answer:**

NPM stands for Node Package Manager. It is used to install, update and manage JavaScript packages and run project scripts.

`node_modules` contains the packages installed for the project, including direct and transitive dependencies.

Common commands:

```bash
npm install
npm install axios
npm run dev
npm run build
```

- `package.json` declares project dependencies and scripts.
- `package-lock.json` records the resolved dependency tree.
- `node_modules` contains the actual installed package files.

Usually, `node_modules` is not committed to source control; it can be recreated using the lock file and `npm install` or `npm ci`.

### Q22. What is the role of public folder in React?

**Answer:**

The `public` folder stores static assets that the build tool serves or copies without processing them as imported source modules.

Examples include:
- `favicon.ico`
- `robots.txt`
- Static images that need stable public paths.
- Files such as a web manifest.

In Vite, a file at `public/logo.png` is typically referenced as `/logo.png`. Assets inside `src` can instead be imported so the build tool can process and fingerprint them.

### Q23. What is the role of src folder in React?

**Answer:**

The `src` folder contains the application's source code. It commonly includes:
- Components.
- Pages.
- Hooks.
- Context providers.
- API/service modules.
- Styles.
- Utility functions.
- The application entry point.

Files inside `src` are processed by the build tool, allowing imports, JSX transformation and bundling.

### Q24. What is the role of index.html page in React? [V. IMP.]

**Answer:**

`index.html` is the browser's initial HTML document. It provides the root DOM element where React mounts the application and includes or references the JavaScript entry bundle.

Example:

```html
<!doctype html>
<html>
  <head>
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

The root element is an ordinary DOM element. React renders the application inside it.

### Q25. What is the role of index.js file and ReactDOM in React? [V. IMP.]

**Answer:**

In many React project templates, `index.js` (or `main.jsx` in Vite) is the entry point. It imports the root component, finds the root DOM element and mounts the React application.

`react-dom` provides APIs for rendering React components into browser DOM nodes.

Modern React uses `createRoot()` from `react-dom/client`:

```jsx
import { createRoot } from "react-dom/client";
import App from "./App";

const rootElement = document.getElementById("root");
createRoot(rootElement).render(<App />);
```

Older applications may use `ReactDOM.render()`, which belongs to the legacy rendering API.

### Q26. What is the role of App.js file in React? [V. IMP.]

**Answer:**

`App.js` or `App.jsx` commonly contains the root application component. It composes the main layout and child components, and may connect routing, context providers or application-level UI.

It is a convention, not a mandatory filename. A React application can use a different root component name and file structure.

### Q27. What is the role of function and return inside App.js?

**Answer:**

A functional component is a JavaScript function that returns a React node describing the UI.

```jsx
function App() {
  return (
    <div>
      <h1>Welcome</h1>
    </div>
  );
}

export default App;
```

- `function App()` defines the component.
- `return` provides the React element tree to render.
- JSX inside the return describes the UI.

A component can also use an implicit return with an arrow function:

```jsx
const App = () => <h1>Welcome</h1>;
```

### Q28. Can we have a function without a return inside App.js?

**Answer:**

A JavaScript function can omit an explicit `return`, but a component must return a valid React node to display UI. If it returns `undefined`, React will report an error for a component that returned nothing; to intentionally render nothing, return `null`.

```jsx
function App() {
  // Some logic
  return null;
}
```

Event-handler or utility functions inside `App` do not need to return UI.

### Q29. What is the role of export default inside App.js?

**Answer:**

`export default` makes one value the default export of a JavaScript module, allowing another file to import it without curly braces.

```jsx
function App() {
  return <h1>Hello</h1>;
}

export default App;
```

Import:

```jsx
import App from "./App";
```

A module can have one default export and multiple named exports. The filename does not have to match the exported component name.

### Q30. Does the file name and component name have to be the same in React?

**Answer:**

No. React does not require the component name and file name to match.

```jsx
// File: Dashboard.jsx
function AdminDashboard() {
  return <h1>Dashboard</h1>;
}

export default AdminDashboard;
```

The convention is to use clear, matching names for easier navigation and maintenance, but it is not a technical requirement.

---

## 4. React – JSX

### Q31. What is the role of JSX in React? [V. IMP.]

**Answer:**

See **Q9** for the core definition. JSX lets developers describe the UI inside JavaScript in a readable, HTML-like syntax. A build tool transforms it into JavaScript that creates React elements.

Example:

```jsx
const name = "Arbaaz";
const element = <h1>Hello, {name}</h1>;
```

Curly braces allow JavaScript expressions inside JSX.

### Q32. What are the 5 advantages of JSX?

**Answer:**

1. **Readable UI:** Markup-like syntax is easier to understand.
2. **JavaScript expressions:** Dynamic values can be embedded using `{}`.
3. **Component composition:** Components can be nested naturally.
4. **Compile-time checks/tooling:** Build tools and editors can detect many syntax and type issues.
5. **Safer text rendering by default:** React escapes text values inserted as children, helping reduce injection risks. Avoid unsafe use of `dangerouslySetInnerHTML`.

### Q33. What is Babel? [V. IMP.]

**Answer:**

Babel is a JavaScript compiler/transpiler toolchain that can transform modern JavaScript syntax and JSX into code supported by the target environment.

For example, JSX:

```jsx
const element = <h1>Hello</h1>;
```

is transformed into JavaScript calls that create React elements (the exact output depends on the JSX runtime and build configuration).

Babel can also transform newer JavaScript syntax using configured plugins and presets. Modern tools such as Vite may use other transformation tools for some tasks, so Babel is not mandatory in every React project.

### Q34. What is the role of Fragment in JSX? [IMP.]

**Answer:**

A Fragment groups multiple elements without adding an extra DOM element.

```jsx
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  );
}
```

The long form is `<React.Fragment>...</React.Fragment>`. Use the long form with a `key` when returning keyed fragments from a list.

Fragments help avoid unnecessary wrapper elements that could affect layout or semantics.

### Q35. What is Spread Operator in JSX?

**Answer:**

The JavaScript spread syntax (`...`) can pass the properties of an object as props to a component.

```jsx
const userProps = {
  name: "Arbaaz",
  role: "Developer"
};

function User({ name, role }) {
  return <p>{name} - {role}</p>;
}

<User {...userProps} />
```

This is equivalent to:

```jsx
<User name="Arbaaz" role="Developer" />
```

Use it carefully: spreading large objects can pass unintended props or make it harder to see which props a component receives.

### Q36. What are the types of Conditional Rendering in JSX? [V. IMP.]

**Answer:**

Common approaches include:

**1. `if/else` before the return:**

```jsx
function Greeting({ loggedIn }) {
  if (loggedIn) {
    return <h1>Welcome back</h1>;
  }
  return <h1>Please log in</h1>;
}
```

**2. Ternary operator:**

```jsx
{loggedIn ? <Dashboard /> : <Login />}
```

**3. Logical AND (`&&`):**

```jsx
{isAdmin && <AdminPanel />}
```

**4. Early return:**

```jsx
if (loading) return <p>Loading...</p>;
return <Dashboard />;
```

Be careful with `count && <Component />`: if `count` is `0`, React may render the number `0`. Use `count > 0 && ...` when a boolean condition is intended.

### Q37. How do you iterate over a list in JSX? What is the map() method? [V. IMP.]

**Answer:**

Use JavaScript's `map()` method to transform each array item into a React element.

```jsx
const users = [
  { id: 1, name: "Aman" },
  { id: 2, name: "Sara" }
];

function UserList() {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

`map()` returns a new array. React uses the `key` prop to identify list items between renders. Prefer stable unique IDs; avoid array indexes as keys when items can be reordered, inserted or removed.

### Q38. Can a browser read a JSX file?

**Answer:**

Browsers do not natively understand JSX syntax. JSX must be transformed into JavaScript by a build tool or transpiler before the browser executes it.

Tools such as Vite, Babel or other JSX-aware compilers perform this transformation during development or build.

### Q39. What is Transpiler? What is the difference between Compiler and Transpiler?

**Answer:**

A **transpiler** transforms source code from one language version or syntax level into another at a similar abstraction level. For example, Babel can transform modern JavaScript or JSX into JavaScript syntax understood by the target environment.

A **compiler** is a broader term for a tool that translates source code into another representation, often machine code, bytecode or another language.

The terms overlap: a transpiler is a type of source-to-source compiler. The distinction is about the kind of output, not a strict universal boundary.

### Q40. Is it possible to use JSX without React?

**Answer:**

Yes. JSX is a syntax extension, not a feature that only React can use. A compiler can transform JSX using a custom JSX runtime or another library's runtime.

However, the common React JSX setup transforms JSX into calls associated with React's JSX runtime. To use JSX without React, configure the compiler's JSX transform for the target library/runtime.

---

## 5. React – Components (Functional/Class)

### Q41. What are React Components? What are the main elements of them? [V. IMP.]

**Answer:**

See **Q5** for the main definition and example. A component is a reusable UI unit. It may receive props, use state and hooks, and return React elements.

A component can be composed with other components to build a complete page or application.

### Q42. What are the types of React Components? What are Functional Components? [V. IMP.]

**Answer:**

Historically, React components are commonly grouped into:

1. **Functional components:** JavaScript functions that return React elements. They can use hooks for state and lifecycle-related behavior.
2. **Class components:** ES6 classes extending `React.Component`, with a `render()` method and lifecycle methods.

Functional components are the recommended default for modern React development.

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

Other informal categories include presentational/container components, controlled/uncontrolled components and higher-order components. These describe patterns rather than separate React component types.

### Q43. How do you pass data between functional components in React? [V. IMP.]

**Answer:**

The method depends on the direction of communication.

**Parent to child:** Pass data through props.

```jsx
function Child({ message }) {
  return <p>{message}</p>;
}

function Parent() {
  return <Child message="Hello from parent" />;
}
```

**Child to parent:** Pass a callback function as a prop, and call it from the child.

```jsx
function Child({ onSend }) {
  return <button onClick={() => onSend("Hello parent")}>Send</button>;
}

function Parent() {
  const handleMessage = message => console.log(message);
  return <Child onSend={handleMessage} />;
}
```

**Sibling to sibling:** Lift shared state to their closest common parent and pass values/callbacks down. For broader sharing, use Context or a state-management solution.

### Q44. What is Prop Drilling in React? [V. IMP.]

**Answer:**

Prop drilling occurs when data is passed through multiple intermediate components as props, even though those components do not need the data themselves.

Example: `App → Layout → Sidebar → UserProfile`, where only `UserProfile` needs the user information but each intermediate component forwards it.

Prop drilling is not always bad. For a short component tree, props make data flow explicit. It becomes harder to maintain when many layers must forward the same data.

### Q45. Why should we avoid Prop Drilling? In how many ways can we avoid Prop Drilling? [IMP.]

**Answer:**

Avoid excessive prop drilling because it can:
- Make intermediate components depend on data they do not use.
- Require many components to change when a prop is renamed or reshaped.
- Make component interfaces noisy.
- Make large component trees harder to maintain.

Common ways to reduce it include:

1. **Component composition:** Pass a component or content through `children` or another slot-like prop.
2. **Context API:** Share values with components within a provider subtree.
3. **State-management libraries:** Redux Toolkit, Zustand or other tools for broader shared state.
4. **Custom hooks:** Encapsulate reusable stateful logic (a custom hook alone does not automatically share one state instance across components).
5. **Lift state appropriately:** Keep state in the nearest common owner and pass only what is needed.

Choose the simplest option. Do not introduce global state for data used by only a few nearby components.

### Q46. What are Class Components in React?

**Answer:**

A class component is an ES6 class that extends `React.Component` and implements a `render()` method.

```jsx
import React from "react";

class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Class components can use state through `this.state`, update it using `this.setState()`, and use lifecycle methods such as `componentDidMount()`.

They are still supported, but functional components with hooks are generally preferred for new code.

### Q47. How do you pass data between Class Components in React?

**Answer:**

The same parent-child data flow applies.

**Parent to child:** Pass props.

```jsx
class Child extends React.Component {
  render() {
    return <p>{this.props.message}</p>;
  }
}

class Parent extends React.Component {
  render() {
    return <Child message="Hello from parent" />;
  }
}
```

**Child to parent:** Pass a callback as a prop and call it from the child.

```jsx
class Child extends React.Component {
  render() {
    return (
      <button onClick={() => this.props.onSend("Hello")}>
        Send
      </button>
    );
  }
}

class Parent extends React.Component {
  handleSend = message => {
    console.log(message);
  };

  render() {
    return <Child onSend={this.handleSend} />;
  }
}
```

For siblings, lift shared state to their common parent or use Context/state management.

### Q48. What is the role of the `this` keyword in Class Components?

**Answer:**

In a class component, `this` refers to the current class instance. It is used to access:
- Props through `this.props`.
- State through `this.state`.
- Methods through `this.methodName`.
- State updates through `this.setState()`.

Event handlers can lose their expected `this` context if passed as an unbound method. Common solutions include binding in the constructor or using class-field arrow functions.

```jsx
class Counter extends React.Component {
  state = { count: 0 };

  handleClick = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return <button onClick={this.handleClick}>{this.state.count}</button>;
  }
}
```

For state updates based on previous state, prefer the functional form of `setState()`.

### Q49. What are the 5 differences between Functional Components and Class Components? [V. IMP.]

**Answer:**

| Functional Components | Class Components |
|---|---|
| JavaScript functions | ES6 classes extending `React.Component` |
| Use hooks such as `useState` and `useEffect` | Use `this.state`, `this.setState()` and lifecycle methods |
| No `this` keyword for component state/props | Use `this.props` and `this.state` |
| Logic can be reused through custom hooks | Logic reuse historically used patterns such as HOCs and render props |
| Commonly preferred in modern React | Still supported, often found in older codebases |

Both can render UI and receive props. Hooks cannot be called inside class components.

---

## 6. React – Routing

### Q50. What is Routing and Router in React? [V. IMP.]

**Answer:**

Routing maps a URL path to the UI that should be displayed. In a React SPA, a client-side router can update the displayed page without requesting a complete new HTML document for every navigation.

A router manages route matching, navigation, URL parameters and related state. React Router is a popular routing library.

### Q51. How to implement Routing in React? [V. IMP.]

**Answer:**

With a modern React Router version:

```bash
npm install react-router-dom
```

Example:

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function Home() {
  return <h1>Home</h1>;
}

function About() {
  return <h1>About</h1>;
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

The hosting server must also be configured to serve the SPA entry document for client-side routes when users directly open or refresh those URLs.

### Q52. What are the roles of `Routes` and `Route` components in React Routing? [V. IMP.]

**Answer:**

In React Router v6 and later:
- `Routes` examines its child route definitions and renders the best matching route branch.
- `Route` defines a URL path and the element to render when it matches.

```jsx
<Routes>
  <Route path="/users" element={<UserList />} />
  <Route path="/users/:id" element={<UserDetails />} />
</Routes>
```

`Route` is used inside `Routes` (or a route configuration); it is not normally rendered directly as a standalone component.

### Q53. What are Route Parameters in React Routing? [IMP.]

**Answer:**

Route parameters are dynamic values embedded in a route path.

```jsx
<Route path="/users/:userId" element={<UserDetails />} />
```

Read the parameter with `useParams()`:

```jsx
import { useParams } from "react-router-dom";

function UserDetails() {
  const { userId } = useParams();
  return <h1>User ID: {userId}</h1>;
}
```

For `/users/42`, `userId` is `"42"` (a string). Validate or convert it before using it as a numeric ID.

### Q54. What is the role of Switch Component in React Routing?

**Answer:**

`Switch` was used in React Router v5 to render the first matching route. In React Router v6 and later, `Switch` was replaced by `Routes`, which uses route ranking to select the best matching route branch.

Legacy v5 example:

```jsx
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
</Switch>
```

Modern v6+ equivalent:

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
```

### Q55. What is the role of the `exact` prop in React Routing?

**Answer:**

In React Router v5, `exact` required a route path to match the URL exactly rather than matching a prefix.

For example, without `exact`, `/` could match paths beginning with `/`, depending on route configuration.

In React Router v6 and later, route matching is ranked and more specific routes are matched appropriately; the `exact` prop is no longer used.

---

## 7. React Hooks – useState/useEffect

### Q56. What are React Hooks? What are the top React Hooks? [V. IMP.]

**Answer:**

Hooks are functions that let functional components use React features such as state, context, refs and effects.

Common hooks:
- `useState`: Local component state.
- `useEffect`: Synchronize with external systems after rendering.
- `useContext`: Read context values.
- `useReducer`: Manage state using a reducer function.
- `useRef`: Hold a mutable value or DOM reference without triggering a render.
- `useMemo`: Cache a calculated value between renders when dependencies are unchanged.
- `useCallback`: Cache a function definition between renders.
- `useLayoutEffect`: Run an effect after DOM mutations but before browser paint.
- `useId`: Generate IDs suitable for accessibility relationships.

Hooks must be called at the top level of function components or custom hooks, not inside conditions, loops or ordinary nested functions.

### Q57. What are State, Stateless, Stateful and State Management terms? [V. IMP.]

**Answer:**

See **Q19** for definitions and a `useState` example.

In short:
- State is changing data that affects UI or behavior.
- Stateful components own/manage state.
- Stateless components derive UI from inputs.
- State management is the overall approach to storing, updating and sharing state.

### Q58. What is the role of `useState()` hook and how does it work? [V. IMP.]

**Answer:**

`useState` adds local state to a functional component.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(previous => previous + 1)}>
      Count: {count}
    </button>
  );
}
```

- `useState(0)` initializes the state to `0`.
- `count` is the current state value for that render.
- `setCount` requests a state update.
- React schedules a new render using the updated value.

State should be treated as immutable. For objects and arrays, create a new object/array instead of mutating the existing state directly. Use the functional updater when the next value depends on the previous value.

### Q59. What is the role of `useEffect()`? How does it work and what is its use? [V. IMP.]

**Answer:**

`useEffect` synchronizes a component with an external system after React commits a render.

Common use cases:
- Subscribing to events or external data sources.
- Setting up and cleaning up timers.
- Connecting to a browser API or third-party widget.
- Fetching data when appropriate (though framework data-loading tools or query libraries are often preferable).

```jsx
import { useEffect, useState } from "react";

function Clock() {
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timerId = setInterval(() => {
      setTime(new Date());
    }, 1000);

    return () => clearInterval(timerId);
  }, []);

  return <p>{time.toLocaleTimeString()}</p>;
}
```

The cleanup function runs before the effect is set up again when dependencies change and when the component is removed. In development, Strict Mode may run an additional setup/cleanup cycle to help reveal unsafe effects.

### Q60. What is Dependency Array in `useEffect()` hook? [V. IMP.]

**Answer:**

The dependency array tells React which reactive values an effect depends on.

```jsx
useEffect(() => {
  console.log(userId);
}, [userId]);
```

When `userId` changes, React runs the cleanup for the previous effect (if present) and then runs the effect again.

The three common forms are:
- No dependency array: effect runs after every committed render.
- Empty array `[]`: effect runs after the initial mount in production, with development Strict Mode behavior noted in Q61.
- Values such as `[userId]`: effect re-runs when one of those values changes.

Include all reactive values used by the effect, following the exhaustive-deps lint rule. Do not use the dependency array merely to force a desired schedule.

### Q61. What is the meaning of the empty array `[]` in the `useEffect()` hook? [V. IMP.]

**Answer:**

An empty dependency array means the effect has no reactive dependencies. It runs after the component's initial mount and its cleanup runs when the component unmounts.

```jsx
useEffect(() => {
  console.log("Component mounted");

  return () => {
    console.log("Component cleanup");
  };
}, []);
```

In development, when Strict Mode is enabled, React may perform an extra setup-and-cleanup cycle to help detect effects that are not correctly cleaned up. Therefore, do not rely on an effect running exactly once in every development environment.

---

## 8. React Hooks – useContext/useReducer

### Q62. What is the role of `useContext()` hook? [V. IMP.]

**Answer:**

`useContext` reads the current value from a React context. It lets components access shared values without manually passing props through every intermediate component.

```jsx
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div className={theme}>Toolbar</div>;
}
```

The component reads the value from the nearest matching provider above it, or the context's default value if no provider exists.

### Q63. What is `createContext()` method? What are Provider and Consumer properties? [IMP.]

**Answer:**

`createContext()` creates a context object used to share a value across a component subtree.

```jsx
import { createContext } from "react";

const ThemeContext = createContext("light");
```

- **Provider:** Supplies a context value to descendant components. In current React versions, the context object can be rendered as a provider; `.Provider` remains widely used and supported.
- **Consumer:** Older render-prop API for reading a context value. Most modern function components use `useContext()` instead.

Example:

```jsx
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

### Q64. When should you use the `useContext` hook instead of props in a real application? [V. IMP.]

**Answer:**

Use props when data is needed by a small number of components and explicit data flow is useful.

Use Context when a value is needed by many components across different levels of a component tree, such as:
- Theme.
- Current authenticated user.
- Locale/language.
- Application-level configuration.

Avoid using Context for every piece of state. A frequently changing large context value can cause many consumers to re-render. Split contexts by responsibility or use a state-management library when the state is complex.

### Q65. What are the similarities between `useState()` and `useReducer()` hooks?

**Answer:**

Both hooks:
- Add state to functional components.
- Return the current state and a function to request updates.
- Trigger a re-render when the state changes.
- Preserve state between renders.
- Should be used without mutating existing state directly.

`useState` is simpler for independent or straightforward state updates. `useReducer` is useful when updates follow multiple related action types or more complex transitions.

### Q66. What is `useReducer()` hook? When should you use `useState()` and when should you use `useReducer()`? [V. IMP.]

**Answer:**

`useReducer` manages state through a reducer function. The reducer receives the current state and an action, then returns the next state.

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return { count: 0 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </>
  );
}
```

**Use `useState` when:**
- State is simple.
- Updates are straightforward.
- A few independent values are managed.

**Use `useReducer` when:**
- State has multiple related fields.
- Several actions update the state.
- Transition logic is complex or should be centralized.
- Reducer logic should be tested independently.

### Q67. What are the differences between `useState()` and `useReducer()` hooks? [V. IMP.]

**Answer:**

| `useState` | `useReducer` |
|---|---|
| Returns current state and setter | Returns current state and `dispatch` |
| Setter receives a value or updater function | Dispatch sends an action to a reducer |
| Good for simple state | Good for complex state transitions |
| Update logic is often near the component | Update logic is centralized in reducer |
| Less setup | Requires a reducer and action design |

Both are built-in React hooks and both can manage objects or arrays immutably.

### Q68. What are dispatch and reducer functions in `useReducer` hook?

**Answer:**

- **Reducer:** A function that receives the current state and an action, then returns the next state.
- **Dispatch:** The function used by a component to send an action to the reducer.

```jsx
function reducer(state, action) {
  if (action.type === "add") {
    return { count: state.count + action.amount };
  }
  return state;
}

const [state, dispatch] = useReducer(reducer, { count: 0 });

dispatch({ type: "add", amount: 2 });
```

The reducer should be pure: it should not mutate state or perform side effects such as API calls.

### Q69. What is the purpose of passing initial state as an object in `useReducer`?

**Answer:**

An object is often used as initial state when the component manages multiple related values.

```jsx
const initialState = {
  name: "",
  email: "",
  loading: false,
  error: null
};

const [state, dispatch] = useReducer(reducer, initialState);
```

This groups related values into one state structure and lets the reducer update them through actions.

For expensive initialization, `useReducer` also supports a third `init` argument:

```jsx
const [state, dispatch] = useReducer(reducer, rawData, createInitialState);
```

The initializer function computes the initial state.

---

## 9. React – Component Lifecycle Methods

### Q70. What are Component Lifecycle Phases? [V. IMP.]

**Answer:**

Class component lifecycle is commonly explained in three broad phases:

1. **Mounting:** The component is created and inserted into the DOM.
2. **Updating:** The component re-renders because props or state changed.
3. **Unmounting:** The component is removed from the DOM.

There are also error-handling lifecycle methods for certain rendering errors.

Functional components do not have class lifecycle methods. Hooks such as `useEffect` provide ways to synchronize with external systems, but they are not a one-to-one replacement for every lifecycle method.

### Q71. What are Component Lifecycle Methods? [V. IMP.]

**Answer:**

Lifecycle methods are special methods available in React class components that run at particular stages of a component's lifecycle.

Important methods include:

- `constructor()`: Initialize state and bind methods when needed.
- `static getDerivedStateFromProps()`: Derive state from props in rare cases.
- `render()`: Return the UI description.
- `componentDidMount()`: Runs after the component mounts.
- `shouldComponentUpdate()`: Can control whether a class component should re-render.
- `getSnapshotBeforeUpdate()`: Capture information from the DOM before an update is committed.
- `componentDidUpdate()`: Runs after an update is committed.
- `componentWillUnmount()`: Cleanup before removal.
- `static getDerivedStateFromError()` and `componentDidCatch()`: Error boundary lifecycle methods.

Some older lifecycle methods are deprecated/unsafe for modern usage.

**Note:** Question 72 was missing in the supplied list.

### Q73. What are Constructors in Class Components? When should you use them?

**Answer:**

A constructor is a JavaScript class method called when an instance is created. In a React class component, it is commonly used to initialize state or bind event-handler methods.

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    return <p>{this.state.count}</p>;
  }
}
```

A constructor is not required if no initialization or binding is needed. Modern class fields can initialize state and arrow-function handlers without a constructor.

### Q74. What is the role of the `super` keyword in a constructor?

**Answer:**

`super(props)` calls the constructor of the parent class, which is `React.Component` for a class component.

In a derived JavaScript class, `this` cannot be used before calling `super()`. Passing `props` also makes them available as `this.props` during the constructor.

```jsx
class User extends React.Component {
  constructor(props) {
    super(props);
    this.state = { active: true };
  }

  render() {
    return <p>{this.props.name}</p>;
  }
}
```

### Q75. What is the role of the `render()` method in the Component Lifecycle?

**Answer:**

In a class component, `render()` returns the React elements describing the UI.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

React calls it during mounting and when the component needs to update. It should be pure: do not perform API calls, mutate state or directly manipulate the DOM inside `render()`.

### Q76. How can the state be maintained in a Class Component?

**Answer:**

Class components maintain local state through `this.state` and update it using `this.setState()`.

```jsx
class Counter extends React.Component {
  state = { count: 0 };

  increment = () => {
    this.setState(previousState => ({
      count: previousState.count + 1
    }));
  };

  render() {
    return (
      <button onClick={this.increment}>
        {this.state.count}
      </button>
    );
  }
}
```

`setState()` schedules an update and shallowly merges the supplied object into the existing state. Use the functional form when the next state depends on the previous state.

### Q77. What is the role of `componentDidMount()` method in the Component Lifecycle? [V. IMP.]

**Answer:**

`componentDidMount()` runs after a class component has been mounted into the DOM.

Common use cases:
- Starting a subscription.
- Setting up a timer.
- Integrating with a non-React DOM library.
- Fetching data in legacy class components.

```jsx
class Users extends React.Component {
  state = { users: [] };

  async componentDidMount() {
    const response = await fetch("/api/users");
    const users = await response.json();
    this.setState({ users });
  }

  render() {
    return <pre>{JSON.stringify(this.state.users)}</pre>;
  }
}
```

If the component creates a subscription or timer, clean it up in `componentWillUnmount()`. In modern functional components, the equivalent synchronization pattern is commonly implemented with `useEffect`.

---

## 10. React – Controlled & Uncontrolled Components

### Q78. What are Controlled Components in React? [V. IMP.]

**Answer:**

A controlled form component is an input whose current value is controlled by React state. The input receives its value through a prop, and an event handler updates the state.

```jsx
function NameForm() {
  const [name, setName] = useState("");

  return (
    <input
      value={name}
      onChange={event => setName(event.target.value)}
    />
  );
}
```

React state is the source of truth for the input value.

### Q79. What are the differences between Controlled and Uncontrolled Components? [V. IMP.]

**Answer:**

| Controlled | Uncontrolled |
|---|---|
| Value is stored in React state | Value is primarily maintained by the DOM |
| Uses `value` or `checked` | Commonly uses `defaultValue` or `defaultChecked` |
| Updates through event handlers | Value can be read through a ref or form submission |
| Easy to validate on each change | Useful for simpler forms or DOM-integrated controls |
| More explicit control | Less state and event-handler code |

### Q80. What are the characteristics of Controlled Components?

**Answer:**

- React state stores the current input value.
- `value` or `checked` is passed to the form element.
- `onChange` updates state.
- Validation can run as the user types or when submitting.
- The UI can respond immediately to input changes.
- The displayed value stays synchronized with React state.

### Q81. What are the advantages of using Controlled Components in React Forms? [IMP.]

**Answer:**

1. Centralized form state.
2. Real-time validation and error messages.
3. Easy conditional UI based on field values.
4. Easy to disable submit until data is valid.
5. Predictable form behavior and easier testing.
6. Straightforward integration with API request payloads.

For very large forms, consider a form library to reduce boilerplate and manage validation efficiently.

### Q82. How to handle Forms in React? [V. IMP.]

**Answer:**

A simple form can use controlled inputs, state and an `onSubmit` handler.

```jsx
import { useState } from "react";

function LoginForm() {
  const [email, setEmail] = useState("");

  function handleSubmit(event) {
    event.preventDefault();
    console.log({ email });
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={event => setEmail(event.target.value)}
        required
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

`preventDefault()` stops the browser's default full-page form submission. The handler can validate the data and call the backend API.

### Q83. How can you handle multiple input fields in a Controlled Form? [IMP.]

**Answer:**

Store the fields in one state object and update the field corresponding to the input's `name`.

```jsx
function SignupForm() {
  const [form, setForm] = useState({
    name: "",
    email: ""
  });

  const handleChange = event => {
    const { name, value } = event.target;
    setForm(previous => ({
      ...previous,
      [name]: value
    }));
  };

  return (
    <>
      <input name="name" value={form.name} onChange={handleChange} />
      <input name="email" value={form.email} onChange={handleChange} />
    </>
  );
}
```

The spread operator preserves the other fields while updating one property.

### Q84. How do you handle Form Validation in a Controlled Component? [V. IMP.]

**Answer:**

Validation can happen while typing, on blur, or when the user submits the form.

Typical validation:
- Required fields.
- Email format.
- Minimum/maximum lengths.
- Password rules.
- Cross-field rules, such as matching passwords.

```jsx
function EmailForm() {
  const [email, setEmail] = useState("");
  const [error, setError] = useState("");

  function handleSubmit(event) {
    event.preventDefault();

    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      setError("Enter a valid email address");
      return;
    }

    setError("");
    // Submit to API
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={email}
        onChange={event => setEmail(event.target.value)}
        type="email"
      />
      {error && <p role="alert">{error}</p>}
      <button type="submit">Submit</button>
    </form>
  );
}
```

Frontend validation improves user experience, but the backend must validate again because clients can be bypassed.

### Q85. In what scenarios might using Uncontrolled Components be advantageous?

**Answer:**

Uncontrolled components can be useful when:
- Integrating with a third-party library that directly manages DOM elements.
- Working with file inputs, whose value is controlled by the browser.
- Building a small/simple form where live React state is unnecessary.
- Migrating a traditional HTML form.
- Reading values only when the form is submitted.

Example:

```jsx
function SearchForm() {
  const inputRef = useRef(null);

  function handleSubmit(event) {
    event.preventDefault();
    console.log(inputRef.current.value);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} defaultValue="" />
      <button type="submit">Search</button>
    </form>
  );
}
```

---

## 11. React – Code Splitting

### Q86. What is Code Splitting in React? [IMP.]

**Answer:**

Code splitting divides an application's JavaScript bundle into smaller chunks that can be loaded when needed instead of downloading all application code at the beginning.

For example, an admin page's code can be loaded only when a user navigates to the admin route.

**Benefits:** Smaller initial download, potentially faster startup and loading code on demand.

### Q87. How to implement Code Splitting in React? [IMP.]

**Answer:**

Use dynamic `import()` with `React.lazy()` and `Suspense`.

```jsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<p>Loading dashboard...</p>}>
      <Dashboard />
    </Suspense>
  );
}
```

The bundler can create a separate chunk for `Dashboard` and load it when React needs to render the lazy component.

Route-level code splitting is a common approach because users do not need every page's code immediately.

### Q88. What is the role of `lazy` and `Suspense` methods in React? [V. IMP.]

**Answer:**

- `lazy()` lets a component be loaded dynamically through a Promise returned by `import()`.
- `Suspense` displays a fallback UI while a supported child is waiting to load.

```jsx
const Profile = lazy(() => import("./Profile"));

<Suspense fallback={<div>Loading...</div>}>
  <Profile />
</Suspense>
```

`lazy()` is a function; `Suspense` is a component. A suitable error boundary should also handle chunk-loading failures.

### Q89. What are the Pros and Cons of Code Splitting?

**Answer:**

**Pros:**
- Reduces initial JavaScript download.
- Loads page-specific code on demand.
- Can improve initial load performance.
- Helps organize large applications into chunks.

**Cons:**
- May introduce loading delays when a user first opens a split route.
- Adds complexity around loading and error states.
- Too many tiny chunks can create network overhead.
- Chunk loading can fail due to network or deployment/version issues.

Use bundle analysis and real performance measurements to decide where to split.

### Q90. What is the role of the `import()` function in Code Splitting?

**Answer:**

`import()` is JavaScript's dynamic import expression. It loads a module asynchronously and returns a Promise.

```javascript
const module = await import("./analytics.js");
module.trackEvent("page_view");
```

Bundlers can use dynamic imports as split points and generate separate chunks. React's `lazy()` uses dynamic import to load components.

### Q91. What is the purpose of the `fallback` prop in Suspense?

**Answer:**

The `fallback` prop defines what React should display while a Suspense-enabled child is not yet ready.

```jsx
<Suspense fallback={<p>Loading...</p>}>
  <Reports />
</Suspense>
```

The fallback can be a spinner, skeleton or loading message. It is replaced by the child UI when the required code or supported resource becomes available.

### Q92. Can you dynamically load CSS files using Code Splitting in React?

**Answer:**

Yes. Modern bundlers such as Vite and webpack can process CSS imported by JavaScript modules and may split CSS associated with dynamically imported chunks.

```jsx
const Reports = lazy(() => import("./Reports"));
```

If `Reports.jsx` imports `Reports.css`, the bundler can arrange for the relevant CSS to be loaded with the chunk, depending on the tool and configuration.

You can also dynamically import a CSS file in supported bundler setups:

```javascript
await import("./reports.css");
```

This is bundler-dependent; it is not a universal browser-native CSS module-loading API.

### Q93. How do you inspect and analyze the generated chunks in a React application?

**Answer:**

Use the build tool's output and bundle-analysis tools.

Typical workflow:
1. Run a production build, such as `npm run build`.
2. Inspect generated JavaScript and CSS files and their sizes.
3. Use a bundle analyzer compatible with the bundler, such as a Vite visualizer plugin or webpack bundle analyzer.
4. Identify large dependencies, duplicated packages and unexpectedly large chunks.
5. Check network requests in browser DevTools to see when chunks load.
6. Measure loading performance using Lighthouse or browser performance tools.

Do not optimize based only on file size; measure actual user-facing performance.

---

## 12. React – Other Important Topics

### Q94. What is a Higher-Order Component (HOC) in React? [IMP.]

**Answer:**

A Higher-Order Component is a function that takes a component and returns a new component with additional behavior or props.

```jsx
function withLoading(Component) {
  return function WrappedComponent({ isLoading, ...props }) {
    if (isLoading) return <p>Loading...</p>;
    return <Component {...props} />;
  };
}

function UserList({ users }) {
  return <ul>{users.map(user => <li key={user.id}>{user.name}</li>)}</ul>;
}

const UserListWithLoading = withLoading(UserList);
```

HOCs were a common logic-reuse pattern. Modern React often uses custom hooks and component composition for similar needs. Avoid mutating the wrapped component and preserve useful display names/refs when necessary.

### Q95. What are the 5 ways to style React Components? Explain Inline Styles.

**Answer:**

Common styling approaches include:

1. **External CSS stylesheets:** Import `.css` files and use `className`.
2. **Inline styles:** Pass a JavaScript object to the `style` prop.
3. **CSS Modules:** Locally scoped class names.
4. **CSS-in-JS libraries:** Such as styled-components or Emotion.
5. **Utility CSS frameworks:** Such as Tailwind CSS.

Inline style example:

```jsx
function Button() {
  const buttonStyle = {
    backgroundColor: "blue",
    color: "white",
    padding: "10px 16px",
    borderRadius: "6px"
  };

  return <button style={buttonStyle}>Save</button>;
}
```

**Inline style characteristics:**
- Uses a JavaScript object.
- CSS property names use camelCase, such as `backgroundColor`.
- Numeric values for many CSS properties are treated as pixel values; some properties are unitless.
- Useful for dynamic styles based on state or props.
- Does not support all CSS features conveniently, such as pseudo-classes and media queries.

### Q96. What is the difference between React and React Native? [V. IMP.]

**Answer:**

| React | React Native |
|---|---|
| Commonly used for web UIs | Used for native mobile applications |
| Renders browser DOM elements | Renders native platform UI components |
| Uses elements such as `div`, `p`, `button` | Uses components such as `View`, `Text`, `Pressable` |
| Styles use CSS or CSS-like tooling | Styles use JavaScript style objects with a supported subset of CSS concepts |
| Runs in a browser environment | Runs on iOS/Android with native platform integration |

Both use React concepts such as components, props, state and hooks. React Native is not simply a website displayed inside a mobile WebView.

### Q97. What is GraphQL?

**Answer:**

GraphQL is a query language for APIs and a runtime for executing those queries against a defined schema.

A client requests the exact fields it needs.

Example query:

```graphql
query {
  user(id: "10") {
    name
    email
  }
}
```

A GraphQL API commonly defines:
- **Schema:** Types and available operations.
- **Query:** Reads data.
- **Mutation:** Changes data.
- **Subscription:** Receives updates, commonly over a persistent transport.

Compared with REST, GraphQL often exposes a schema-driven endpoint where clients select fields. It can reduce over-fetching/under-fetching, but caching, authorization, query complexity and N+1 resolver problems require careful design.

### Q98. What are the top 3 ways to achieve State Management? When should you use what in React? [V. IMP.]

**Answer:**

Three common approaches are:

1. **Local component state (`useState` / `useReducer`):** Use when data belongs to one component or a small local component group.
2. **Context API:** Use for values needed across a subtree, such as theme, locale or authenticated user information.
3. **External state-management libraries:** Use tools such as Redux Toolkit or Zustand when shared state is complex, widely accessed or needs structured update patterns and debugging.

Also distinguish **server state** (data fetched from an API) from client UI state. Libraries such as TanStack Query are designed for server-state fetching, caching, synchronization and invalidation; they are not simply replacements for all client state.

Choose based on the scope and complexity of the data rather than adopting a global store by default.

### Q99. How can you implement Authentication in a React application? [V. IMP.]

**Answer:**

React handles the client-side authentication experience, while the backend must enforce authentication and authorization.

Typical flow:
1. User submits credentials through a login form.
2. React sends them over HTTPS to the backend.
3. Backend verifies credentials and issues a session or access token.
4. React stores/uses the authentication state according to the chosen security design.
5. Protected UI routes check authentication state for user experience.
6. API requests include the required session cookie or bearer token.
7. Backend validates the session/token and permissions on every protected request.

**Security considerations:**
- Prefer secure, HttpOnly, Secure and appropriately configured SameSite cookies for browser sessions when suitable.
- If bearer tokens are used, carefully assess storage and XSS risks; long-lived sensitive tokens in `localStorage` are exposed to JavaScript.
- Protect cookie-based state-changing requests against CSRF where applicable.
- Use HTTPS.
- Do not rely on hiding a React route as the only authorization check.
- Handle expiry, logout and refresh securely.

### Q100. What is the use of React Profiler? [IMP.]

**Answer:**

React Profiler is a developer tool/API used to measure rendering performance and identify components that render and how much time rendering takes.

The React DevTools Profiler can:
- Record render activity.
- Show which components rendered.
- Display render duration.
- Help identify expensive component trees.
- Compare commits and investigate unnecessary renders.

React also provides a `<Profiler>` component for programmatic profiling in supported profiling builds:

```jsx
import { Profiler } from "react";

function onRender(id, phase, actualDuration) {
  console.log({ id, phase, actualDuration });
}

<Profiler id="Dashboard" onRender={onRender}>
  <Dashboard />
</Profiler>
```

Use profiling to find measured bottlenecks before applying memoization.

### Q101. What is the difference between Fetch and Axios for API calls in React? [V. IMP.]

**Answer:**

Fetch is a browser-native API. Axios is a third-party HTTP client library.

| Fetch | Axios |
|---|---|
| Built into modern browsers | Must be installed |
| Does not reject for HTTP error statuses such as 404 by default | Rejects non-2xx responses by default |
| Response body must be parsed, e.g. `response.json()` | Automatically transforms common JSON responses |
| Supports `AbortController` | Supports cancellation using `AbortController` in modern versions |
| Interceptors need to be implemented separately | Provides request/response interceptors |
| Lightweight, no additional dependency | More built-in conveniences |

Fetch example:

```javascript
const response = await fetch("/api/users");

if (!response.ok) {
  throw new Error(`HTTP error: ${response.status}`);
}

const users = await response.json();
```

Axios example:

```javascript
import axios from "axios";

const response = await axios.get("/api/users");
const users = response.data;
```

Both are valid. Choose based on project needs and existing conventions.

### Q102. What are the popular Testing Libraries for React? [IMP.]

**Answer:**

Common React testing tools include:

- **React Testing Library:** Tests components through user-observable behavior.
- **Vitest:** Test runner commonly used with Vite.
- **Jest:** JavaScript test runner used in many React projects.
- **Playwright:** Browser-based end-to-end testing.
- **Cypress:** End-to-end and component testing.
- **MSW (Mock Service Worker):** Mocks network requests at the network layer for tests.

A common setup is React Testing Library with Vitest or Jest. Prefer testing what users see and do rather than testing internal component implementation details.

### Q103. How can you optimize performance in a React application? [V. IMP.]

**Answer:**

Use profiling and measurements to identify the actual bottleneck. Common techniques include:

1. **Component design:** Keep state close to where it is needed and split large components appropriately.
2. **Memoization:** Use `React.memo`, `useMemo` and `useCallback` when they prevent measured unnecessary work.
3. **Code splitting:** Load route/page chunks on demand.
4. **List rendering:** Use stable keys and virtualization for very large lists.
5. **Avoid unnecessary state:** Derive values from existing props/state when possible.
6. **Optimize assets:** Compress images, use suitable formats and lazy-load off-screen assets.
7. **API efficiency:** Avoid duplicate requests, cache server data and paginate large responses.
8. **Production build:** Serve optimized production assets with compression and caching.
9. **Avoid expensive render work:** Move heavy calculations out of repeated render paths or use workers/server-side processing where appropriate.
10. **Measure:** Use React Profiler, browser DevTools and performance metrics.

Do not add memoization everywhere. It has its own costs and is most useful when it avoids meaningful repeated work.

### Q104. Explain Reactive Programming with an example.

**Answer:**

Reactive programming is a programming approach in which a system responds to changes in data or events. Instead of manually updating every dependent UI element, the application models data/state and reacts when it changes.

In React, a component re-renders when its state or relevant context/props change.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(value => value + 1)}>
      Count: {count}
    </button>
  );
}
```

When the user clicks the button:
1. The event handler requests a state update.
2. React schedules a render.
3. The component reads the new `count`.
4. React updates the UI to show the new value.

React is declarative and reactive to state changes, but it is not itself the same thing as a general-purpose Reactive Extensions (Rx) stream library.

### Q105. In how many ways can we implement Reactive Programming in React?

**Answer:**

There is no fixed official number of ways. Common patterns for building reactive behavior include:

1. **Local state:** `useState` or `useReducer` updates cause the component to render again.
2. **Props:** A parent passes changed data to children.
3. **Context:** Consumers respond when the provided context value changes.
4. **External state stores:** Libraries such as Redux Toolkit or Zustand notify subscribed components about state updates.
5. **Server-state libraries:** Tools such as TanStack Query update subscribed UI when cached server data changes.
6. **Event/observable streams:** Libraries such as RxJS can model streams of events and values, with React integration through subscriptions or hooks.

Choose the approach based on whether the data is local UI state, shared client state, remote server state or an event stream.

### Q106. How to pass data from Child Component to Parent Component in React? [V. IMP.]

**Answer:**

React's normal data flow is parent to child through props. To send information from a child to a parent, the parent passes a callback function to the child. The child calls that function with the data.

```jsx
function Child({ onSendMessage }) {
  const message = "Hello from child";

  return (
    <button onClick={() => onSendMessage(message)}>
      Send to Parent
    </button>
  );
}

function Parent() {
  const [message, setMessage] = useState("");

  return (
    <div>
      <Child onSendMessage={setMessage} />
      <p>Message from child: {message}</p>
    </div>
  );
}
```

**Flow:**
1. Parent defines a callback (here, the state setter).
2. Parent passes it to the child as a prop.
3. Child invokes the callback with a value.
4. Parent updates its state.
5. React renders the parent with the new value.

This is often described as lifting state up and maintaining one-way data flow.
