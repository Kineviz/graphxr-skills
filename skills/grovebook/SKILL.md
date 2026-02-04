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

## Cells

Cells store metadata in an HTML comment in the line preceding the code fence. The format is:

```html
<!--{"pinCode":false,"dname":"social-network-graph-cell","codeMode":"javascript2","hide":false}-->
```

The cell starts with a `{` and ends with a `}`.

The cell can return:
- A primitive JavaScript value
- HTML with an HTML literal like `html\`\``
- Markdown with the `md` literal like `md\`\``
- Any component from ObservableHQ's Inputs library
- A built-in Grove extended library component
- `Button("Label", async() => {})`

## Markdown

Grovebooks are markdown files, so standard markdown syntax can be used throughout the file for text content, headings, lists, links, and other markdown elements.

## Grove Library

<!-- User will fill in this section -->

## Examples

<!-- User will fill in this section -->

### Hello, world!

```html
<!--{"pinCode":false,"dname":"07120854-6c51-47b9-a083-0767ebdae917","codeMode":"javascript2","hide":false}-->
```js
{
  return Button("Hello, world!", async () => {
    console.log("Hello, world!");
  });
}
```
