---
name: grovebook
description: Create and work with grovebook files - markdown files with interactive cells that can return JavaScript values, HTML, markdown, ObservableHQ Inputs components, or Grove extended library components.
---

# Grovebook

## When to use this skill

Use this skill when the user wants to:
- Create or edit grovebook files
- Understand the structure and syntax of grovebooks
- Work with grovebook cells and their metadata
- Generate grovebook content with cells, markdown, and interactive components

## Overview

A grovebook file is a markdown file that contains interactive cells. These cells can execute JavaScript code and return various types of content including primitive values, HTML, markdown, ObservableHQ Inputs components, or Grove extended library components.

## File Extension

Grovebooks MUST end with the .md extension.

## Cells

A cell looks like:

````
<!--{"pinCode":false,"dname":"js-cell","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```
````

### Schema of a cell

1. HTML comment with JSON metadata
2. Code fence with the code for the cell

### Cell metadata

The metadata is a JSON object contained in an HTML comment that precedes the code fence for the cell. It contains the following properties:

- `pinCode`: boolean, whether to pin the cell. If true, the code will display in Grove.
- `dname`: string, unique identifier of the cell.
- `codeMode`: string, the code mode of the cell. Typical values are `js`, `jsx`, and `md`.
- `hide`: boolean, whether to hide the cell while not editing.

Example:
```
<!--{"pinCode":false,"dname":"js-cell","codeMode":"js","hide":false}-->
```


### Types of cells

#### Renderable content

##### HTML

````
<!--{"pinCode":false,"dname":"html-cell","codeMode":"js","hide":false}-->
```js
{
  return html`${1 + 1}`;
}
```
````

##### Markdown with the `md` literal like `{ return md\`\` }`

````
<!--{"pinCode":false,"dname":"markdown-cell","codeMode":"js","hide":false}-->
```js
{
  return md`# Hello, world`;
}
```
````

##### ObservableHQ's Inputs library

See `references/observablehq-inputs.md` for more information about the ObservableHQ Inputs library. One of the following:

```js
Inputs.button(content, options)      // Click trigger
Inputs.toggle(options)               // On/off switch
Inputs.checkbox(data, options)       // Multiple selection
Inputs.radio(data, options)          // Single selection (visible)
Inputs.range(extent, options)        // Number slider
Inputs.number(extent?, options)      // Number input only
Inputs.select(data, options)         // Dropdown menu
Inputs.text(options)                 // Single-line text
Inputs.email(options)                // Email input
Inputs.tel(options)                  // Telephone input
Inputs.url(options)                  // URL input
Inputs.password(options)             // Password input
Inputs.textarea(options)             // Multi-line text
Inputs.date(options)                 // Date picker
Inputs.datetime(options)             // Date + time picker
Inputs.color(options)                // Color picker
Inputs.file(options)                 // File upload
```

````
<!--{"pinCode":false,"dname":"my-cell","codeMode":"js","hide":false}-->
```js
viewof name = Inputs.text({label: "Name", placeholder: "What's your name?"});
```
````

##### JSX

###### A custom React component

- Note that the code mode must be `jsx` for custom React components.
- One component per cell.
- Component definitions and calls to react() must be in separate cells.

````
<!--{"pinCode":false,"dname":"jsx-cell","codeMode":"jsx","hide":false}-->
```jsx
function CounterComponent() {
  const [x, setX] = React.useState(0);
  return <button onClick={() => {
    setX(x + 1);
  }}>{x}</button> 
}
```

<!--{"pinCode":false,"dname":"render-react-cell","codeMode":"jsx","hide":false}-->
```jsx
react(<CounterComponent />);
```
````

###### Using AntD

- See `references/examples/antd-grovebook-example.md` for an example of using AntD in a grovebook.
- Grove provides AntD 4.16.13.
- One component per cell.
- Component definitions and calls to react() must be in separate cells.

````
<!--{"pinCode":false,"dname":"antd-cell","codeMode":"jsx","hide":false}-->
```jsx
function CounterComponent() {
  const {Button} = Antd;
  const [x, setX] = React.useState(0);
  return <Button onClick={() => {
    setX(x + 1);
  }}>{x}</Button> 
}
```

<!--{"pinCode":false,"dname":"render-antd-cell","codeMode":"jsx","hide":false}-->
```jsx
react(<CounterComponent />);
```
````

#### Reactive variable

##### Simple reactive variable

````
<!--{"pinCode":false,"dname":"reactive-variable-cell1","codeMode":"js","hide":false}-->
```js
x = {
  return 1 + 1;
}
```

<!--{"pinCode":false,"dname":"reactive-variable-cell2","codeMode":"js","hide":false}-->
```js
y = {
  return x + 2;
}
```
````

##### viewof

A `viewof` cell creates a link between a user interface element and its value by defining two variables in the DAG: one for the HTML input itself and one for its live data. When a user interacts with the input, the value variable updates automatically, triggering a reactive update of all downstream cells.

````
<!--{"pinCode":false,"dname":"viewof-example","codeMode":"js","hide":false}-->
```js
viewof size = Inputs.range([0, 100], {label: "Size"})
```

<!--{"pinCode":false,"dname":"area-cell","codeMode":"js","hide":false}-->
```js
// Automatically re-runs whenever the slider moves
area = Math.PI * (size ** 2)
```
````

## GraphXR API

Grovebooks use GraphXR API to interact with the GraphXR platform.

Visit https://graphxr.dev/docs/graphxr-api/reference for a detailed reference of the GraphXR API.

## Grove Library

Grove includes a rich library of built-in variables and components.

### Built-in variables

- `width`: number, the width of the current cell in pixels.
- `selectedNodes`: IterableNodes (GraphXR API type), a reactive variable for the nodes that are currently selected in the graph.
- `selectedEdges`: IterableEdges (GraphXR API type), a reactive variable for the edges that are currently selected in the graph.
- `graph`: GraphSelection (GraphXR API type), a reactive variable for the canvas graph.
- `now`: number, a reactive variable for the current time in milliseconds. Imported from `@observablehq/stdlib/src/now.js`
- `uuidv4`: fucntion, UUID v4 generator (uuid ^8.3.2). e.g. `uuidv4()`

### Visualization libraries

- `Plot`: Observable Plot v0.6.14
- `d3`: D3.js v7.8.5 (includes d3-hexbin v0.2.2)
- `L`: Leaflet v1.9.3
- `mermaid`: Mermaid v9.2.2
- `dot`: Graphviz (@observablehq/graphviz v0.2.1)
- `svg`: SVG utilities (from @observablehq/stdlib v3.4.1)
- `vl`: Vega-Lite v5.6.0 (uses Vega v5.22.1, vega-lite-api v5.0.0)

### Utilities

- `_`: Lodash v4.17.21

### React & UI components

- `Antd`: Ant Design v4.16.13
- `AntdIcons`: @ant-design/icons ^5.0.1
- `React`: React ^17.0.1
- `react`: React wrapper function used for rendering React components.

## Examples

### Hello, world!

````
<!--{"pinCode":false,"dname":"07120854-6c51-47b9-a083-0767ebdae917","codeMode":"js","hide":false}-->
```js
{
  return await Button("Hello, world!", async () => {
    console.log("Hello, world!");
  });
}
```
````

### Multiple cells + interleaved Markdown

````
# Hello, world!

<!--{"pinCode":false,"dname":"07120854-6c51-47b9-a083-0767ebdae917","codeMode":"js","hide":false}-->
```js
{
  return await Button("Hello, world!", async () => {
    console.log("Hello, world!");
  });
}
```

## Here's another cell which computes a primitive value.

<!--{"pinCode":false,"dname":"reacting-to-graph-data-change-events","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```

This is the bottom of the file.
````


### A cell which returns a combination of components

````
<!--{"pinCode":false,"dname":"07120854-6c51-47b9-a083-0767ebdae917","codeMode":"js","hide":false}-->
```js
{
  return html`${await Button("Hello, world!", async () => {
    console.log("Hello, world!");
  })} ${await Button("Hello, world!", async () => {
    console.log("Hello, world!");
  })} ${Inputs.button("Hi")}`;
}
```
````

## State Management

In **Grove**, the **Directed Acyclic Graph (DAG)** is the architectural foundation of its reactive runtime. Unlike traditional linear notebooks, where code execution depends on the order of cells on the page, Grove treats each cell as a node in a dataflow graph.

### Key Characteristics of the Grove DAG
*   **Dependency-Based Execution:** Cells are executed based on their topological order. If Cell B references a variable defined in Cell A, a directed edge is created from A to B (\(A \to B\)). Cell B will only run after Cell A has successfully evaluated.
*   **Reactivity:** When a value in a "parent" cell changes (e.g., via an input slider), the Grove runtime automatically propagates that change through the DAG, re-evaluating only the "downstream" cells.
*   **Acyclic Constraint:** The graph must be "acyclic." Circular dependencies (e.g., Cell A depends on B, which depends on A) are prohibited and will cause the Grove runtime to throw an error.
*   **Order Independence:** Because the DAG determines execution, the physical position of cells in Grove does not matter. A variable defined at the bottom of the page can be used at the top; the runtime handles the sequence correctly.

### Implicit Handling of Promises and Generators
The Grove DAG also manages asynchronous state. If a cell returns a **Promise**, downstream cells in the DAG implicitly wait for it to resolve. If a cell is a **Generator**, the DAG triggers a re-evaluation of all dependent cells every time a new value is yielded.

### State Management Examples

#### Basic reactive variable

````
<!--{"pinCode":false,"dname":"reactive-variable-cell1","codeMode":"js","hide":false}-->
```js
x = {
  return 1 + 1;
}
```

<!--{"pinCode":false,"dname":"reactive-variable-cell2","codeMode":"js","hide":false}-->
```js
y = {
  return x + 2;
}
```
````

#### Reactive variable with a promise

````
<!--{"pinCode":false,"dname":"reactive-variable-cell3","codeMode":"js","hide":false}-->
```js
z = {
  return await fetch("https://api.example.com/data");
}
```

<!--{"pinCode":false,"dname":"reactive-variable-cell4","codeMode":"js","hide":false}-->
```js
w = {
  return (await z).json();
}
```
````

#### viewof

A cell which declares a viewof variable is a reactive variable.

````
<!--{"pinCode":false,"dname":"reactive-variable-cell5","codeMode":"js","hide":false}-->
```js
viewof x = Inputs.text({label: "Name", placeholder: "What's your name?"});
```
````

### mutable variable

A ⁠mutable cell allows you to break the standard functional flow of the DAG by creating a variable that can be updated via assignment from other cells. While normal cells are "read-only" to their neighbors, a mutable variable provides a controlled way to handle state that changes in response to events or specific logic.

````
<!--{"pinCode":false,"dname":"mutable-variable-cell1","codeMode":"js","hide":false}-->
```js
mutable counter = 0
```

<!--{"pinCode":false,"dname":"mutable-variable-cell2","codeMode":"js","hide":false}-->
```js
Button("Increment", async () => {
    mutable counter = counter + 1;
});
```
````

## Best Practices

- Prefer AntD components.