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

Grovebooks must end with the .md extension.

## Cells

Cells store metadata in an HTML comment in the line preceding the code fence. The format is:

A cell looks like:

````
<!--{"pinCode":false,"dname":"js-cell","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```
````

Everything from the HTML comment to the closing code fence.

### Types of cells

#### Reactive variable

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

#### A primitive JavaScript value

````
<!--{"pinCode":false,"dname":"primitive-cell","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```
````

#### HTML

````
<!--{"pinCode":false,"dname":"html-cell","codeMode":"js","hide":false}-->
```js
{
  return html`${1 + 1}`;
}
```
````

#### Markdown with the `md` literal like `{ return md\`\` }`

````
<!--{"pinCode":false,"dname":"markdown-cell","codeMode":"js","hide":false}-->
```js
{
  return md`# Hello, world`;
}
```
````

#### ObservableHQ's Inputs library

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

#### A built-in Grove extended library component

````
<!--{"pinCode":false,"dname":"button-cell","codeMode":"js","hide":false}-->
```js
{
  return await Button("Label", async() => {
    console.log("Button clicked");
  });
}
```
````

#### JSX

##### A custom React component

Note that the code mode must be `jsx` for custom React components.

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
````

##### Rendering a custom React component

````
<!--{"pinCode":false,"dname":"jsx-cell","codeMode":"jsx","hide":false}-->
```jsx
react(<CounterComponent />);
```
````

##### Using AntD

Grove provides AntD 4.16.13.

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
````

## Markdown

Grovebooks are markdown files, so standard markdown syntax can be used throughout the file for text content, headings, lists, links, and other markdown elements.

## Grove Library

<!-- User will fill in this section -->

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