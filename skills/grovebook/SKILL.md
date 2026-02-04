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
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```
````

Everything from the HTML comment to the closing code fence.

### Valid return values

#### A primitive JavaScript value

````
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"js","hide":false}-->
```js
{
  return 1 + 1;
}
```
````

#### HTML

````
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"js","hide":false}-->
```js
{
  return html`${1 + 1}`;
}
```
````

#### Markdown with the `md` literal like `{ return md\`\` }`

````
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"js","hide":false}-->
```js
{
  return md`# Hello, world`;
}
```
````

#### ObservableHQ's Inputs library

#### A built-in Grove extended library component

````
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"js","hide":false}-->
```js
{
  return await Button("Label", async() => {

  });
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
  return Button("Hello, world!", async () => {
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
  return Button("Hello, world!", async () => {
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
