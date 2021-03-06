---
layout: docs
title: Clipboard Module
permalink: /docs/modules/clipboard/
---

The Clipboard is handles copy, cut and paste between Quill and external applications. A set of defaults exist to provide sane interpretation of pasted content, with the ability for further customization through matchers.

The Clipboard interprets pasted HTML by traversing the corresponding DOM tree in [post-order](https://en.wikipedia.org/wiki/Tree_traversal#Post-order), building up a [Delta](/guides/working-with-deltas/) representation of all subtrees. At each descendant node, matcher functions are called with the DOM Node and Delta interpretation so far, allowing the matcher to return a modified Delta interpretation.

Familiarity and comfort with [Deltas](https://github.com/ottypes/rich-text) is necessary in using matchers. See [Working with Deltas](/guides/working-with-deltas/) for a starter guide.


### addMatcher

Adds a custom matcher to the Clipboard. Matchers using `nodeType` are called first, in the order they were added, followed by matchers using a CSS `selector`, also in the order they were added.

**Methods**

- `addMatcher(selector, matcher)`
- `addMatcher(nodeType, matcher)`


**Parameters**

| Parameter  | Type       | Description
|------------|------------|------------
| `selector` | _String_   | CSS selector
| `nodeType` | _Number_   | [nodeType](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType) Can be `Node.ELEMENT_NODE` or `Node.TEXT_NODE`
| `matcher`  | _Function_ | Function with signature `function(node: Node, delta: Delta): Delta`

**Examples**

```javascript
quill.clipboard.addMatcher(Node.TEXT_NODE, function(node, delta) {
  return new Delta().insert(node.data);
});

// Interpret a <b> tag as bold
quill.clipboard.addMatcher('B', function(node, delta) {
  return delta.compose(new Delta().retain(delta.length(), { bold: true }));
});
```


## Configuration

An of array matchers can be passed into Clipboard's configuration options. These will be appended after Quill's own default matchers.

```javascript
var quill = new Quill('#editor', {
  modules: {
    clipboard: {
      matchers: [
        ['B', customMatcherA],
        [Node.TEXT_NODE, customMatcherB]
      ]
    }
  }
});
```



