---
id: cheatsheet
title: Cheatsheet
---

## Basics

### Rendering a Component

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

**Keys** help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

**Lifting State Up**: Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let’s see how this works in action.

**Context API**: Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or preferred language.

**Fragments**: A common pattern in React is for a component to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.

## The Component Lifecycle

- Mounting:
  These methods are called in the following order when an instance of a component is being created and inserted into the DOM:

  - constructor()
  - static getDerivedStateFromProps()
  - render()
  - componentDidMount()

- Updating:
  An update can be caused by changes to props or state. These methods are called in the following order when a component is being re-rendered:

  - static getDerivedStateFromProps(props, state)
  - shouldComponentUpdate()
  - render()
  - getSnapshotBeforeUpdate(prevProps, prevState)
  - componentDidUpdate(prevProps, prevState, snapshot)

- Unmounting: This method is called when a component is being removed from the DOM:

  - componentWillUnmount()

- Error Handling:
  These methods are called when there is an error during rendering, in a lifecycle method, or in the constructor of any child component.

  - static getDerivedStateFromError(error)
  - componentDidCatch()

## Code-Splitting

### Bundling

- Most React apps will have their files “bundled” using tools like Webpack, Rollup or Browserify. Bundling is the process of following imported files and merging them into a single file: a “bundle”. This bundle can then be included on a webpage to load an entire app at once.
- **Code-Splitting** is a feature supported by bundlers like Webpack, Rollup and Browserify (via factor-bundle) which can create multiple bundles that can be dynamically loaded at runtime. Code-splitting your app can help you “lazy-load” just the things that are currently needed by the user, which can dramatically improve the performance of your app.

  - **import()**
    The best way to introduce code-splitting into your app is through the dynamic import() syntax.

    Before

    ```javascript
    import { add } from "./math";
    console.log(add(16, 26));
    ```

    After

    ```javascript
    import("./math").then((math) => {
      console.log(math.add(16, 26));
    });
    ```

  - The **React.lazy** function lets you render a dynamic import as a regular component.

    ```javascript
    const OtherComponent = React.lazy(() => import("./OtherComponent"));
    function MyComponent() {
      return (
        <div>
          <Suspense fallback={<div>Loading...</div>}>
            <OtherComponent />
          </Suspense>
        </div>
      );
    }
    ```

## Forwarding Refs

Ref forwarding is a technique for automatically passing a ref through a component to one of its children. This is typically not necessary for most components in the application. However, it can be useful for some kinds of components, especially in reusable component libraries. The most common scenarios are described below.

```javascript
function FancyButton(props) {
  return <button className="FancyButton">{props.children}</button>;
}
```

```javascript
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

## Higher Order Component

A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature.

Concretely, a higher-order component is a function that takes a component and returns a new component.

```javascript
const MainComponent = () => <div> Main Component</div>;
const enhance = (MyComponent) => {
  return class WrappedComponent extends React.Component {
    componentDidMount() {
      console.log("WrappedComponent Mounted");
    }
    render() {
      return <MyComponent {...this.props} />;
    }
  };
};
const HOC = enhance(MainComponent);

const App = () => (
  <div>
    <p>App</p>
    <HOC />
  </div>
);
```

## JSX In Depth

Fundamentally, JSX just provides syntactic sugar for the React.createElement(component, props, ...children) function. The JSX code:

```javascript
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

```javascript
React.createElement(MyButton, { color: "blue", shadowSize: 2 }, "Click Me");
```

## Optimizing Performance

- Use the Production Build
- Virtualize Long Lists
- Avoid Reconciliation: shouldComponentUpdate, React.PureComponent.
- memoization.

## Portals

Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```javascript
ReactDOM.createPortal(child, container);
```

The first argument (child) is any renderable React child, such as an element, string, or fragment. The second argument (container) is a DOM element.

A typical use case for portals is when a parent component has an overflow: hidden or z-index style, but you need the child to visually “break out” of its container. For example, dialogs, hovercards, and tooltips.

## Reconciliation

When a component’s props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM. This process is called “reconciliation”.

React provides a declarative API so that you don’t have to worry about exactly what changes on every update. This makes writing applications a lot easier, but it might not be obvious how this is implemented within React. This article explains the choices we made in React’s “diffing” algorithm so that component updates are predictable while being fast enough for high-performance apps.

React implements a heuristic O(n) algorithm based on two assumptions:

- Two elements of different types will produce different trees.
- The developer can hint at which child elements may be stable across different renders with a key prop.

An algorithm is the description of an automated solution to a problem. What the algorithm does is precisely defined. The solution could or could not be the best possible one but you know from the start what kind of result you will get. You implement the algorithm using some programming language to get (a part of) a program.

```
Now, some problems are hard and you may not be able to get an acceptable solution in an acceptable time. In such cases you often can get a not too bad solution much faster, by applying some arbitrary choices (educated guesses): that's a heuristic.

A heuristic is still a kind of an algorithm, but one that will not explore all possible states of the problem, or will begin by exploring the most likely ones.

Typical examples are from games. When writing a chess game program you could imagine trying every possible move at some depth level and applying some evaluation function to the board. A heuristic would exclude full branches that begin with obviously bad moves
```

## What is React memo function

Class components can be restricted from rendering when their input props are the same using PureComponent or shouldComponentUpdate. Now you can do the same with function components by wrapping them in React.memo.

```javascript
const MyComponent = React.memo(function MyComponent(props) {
  /* only rerenders if props change */
});
```

## Render Props

The term “render prop” refers to a technique for sharing code between React components using a prop whose value is a function.

A component with a render prop takes a function that returns a React element and calls it instead of implementing its own render logic.

```javascript
<DataProvider render={(data) => <h1>Hello {data.target}</h1>} />
```

Libraries that use render props include React Router, Downshift and Formik.

It’s important to remember that just because the pattern is called “render props” you don’t have to use a prop named render to use this pattern. In fact, any prop that is a function that a component uses to know what to render is technically a “render prop”.

  <details>
      <summary>Example</summary>
      
      ```javascript

        class Mouse extends React.Component {
          constructor(props) {
          super(props);
          this.handleMouseMove = this.handleMouseMove.bind(this);
          this.state = { x: 0, y: 0 };
        }

        handleMouseMove(event) {
          this.setState({
            x: event.clientX,
            y: event.clientY
          });
        }

        render() {
          return (
            <div style={{ height: "100%" }} onMouseMove={this.handleMouseMove}>
              {/*
                Instead of providing a static representation of what <Mouse> renders,
                use the `render` prop to dynamically determine what to render.
              */}
              {this.props.render(this.state)}
            </div>
          );
        }
      }

      class MouseTracker extends React.Component {
        render() {
          return (
            <div>
              <h1>Move the mouse around!</h1>
              <Mouse render={mouse => <Cat mouse={mouse} />} />
            </div>
          );
        }
      }

      export default MouseTracker;
    ```

</details>

## ReactDOM

The react-dom package provides DOM-specific methods that can be used at the top level of your app and as an escape hatch to get outside of the React model if you need to. Most of your components should not need to use this module.

- render() : Render a React element into the DOM in the supplied container and return a reference to the component (or returns null for stateless components). If the React element was previously rendered into container, this will perform an update on it and only mutate the DOM as necessary to reflect the latest React element.

- hydrate() : Same as render(), but is used to hydrate a container whose HTML contents were rendered by ReactDOMServer. React will attempt to attach event listeners to the existing markup. React expects that the rendered content is identical between the server and the client. It can patch up differences in text content, but you should treat mismatches as bugs and fix them. In development mode, React warns about mismatches during hydration.

- unmountComponentAtNode()
- findDOMNode()
- createPortal()

## ReactDOMServer

The ReactDOMServer object enables you to render components to static markup. Typically, it’s used on a Node server:

The following methods can be used in both the server and browser environments:

- ReactDOMServer.renderToString(element): Render a React element to its initial HTML. React will return an HTML string. You can use this method to generate HTML on the server and send the markup down on the initial request for faster page loads and to allow search engines to crawl your pages for SEO purposes. If you call ReactDOM.hydrate() on a node that already has this server-rendered markup, React will preserve it and only attach event handlers, allowing you to have a very performant first-load experience.

## SyntheticEvent

Your event handlers will be passed instances of SyntheticEvent, a cross-browser wrapper around the browser’s native event. It has the same interface as the browser’s native event, including stopPropagation() and preventDefault(), except the events work identically across all browsers.

**Event Pooling**: The SyntheticEvent is pooled. This means that the SyntheticEvent object will be reused and all properties will be nullified after the event callback has been invoked. This is for performance reasons. As such, you cannot access the event in an asynchronous way.

## Redux

What are the core principles of Redux?

- Single source of truth
- State is read-only
- Changes are made with pure functions

mapStateToProps: Is a utility which helps your component get updated state

mapDispatchToProps: Is a utility which will help your component to fire an action event
