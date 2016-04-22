## Formatting

### format

Format text at user's current selection. If the user's selection length is 0, i.e. it is a cursor, the format will be set active, so the next character the user types will have that formatting.

**Methods**

- `format(name, value)`
- `format(name, value, source)`

**Parameters**

| Parameter | Type     | Description
|-----------|----------|------------
| `name`    | _String_ | Name of format to apply.
| `value`   | _String_ | Value of format to apply. A falsy value will remove the format.
| `source`  | _String_ | [Source](/docs/api/#text-change) to be emitted. Defaults to `api`.

**Examples**

{% highlight javascript %}
editor.format('color', 'red');
editor.format('align', 'right');
{% endhighlight %}


### formatLine

Formats all lines in given range. See [formats](/docs/formats/) for a list of available formats. Has no effect when called with inline formats. The user's selection may not be preserved.

**Methods**

- `formatLine(index, length)`
- `formatLine(index, length, name, value)`
- `formatLine(index, length, formats)`
- `formatLine(index, length, source)`
- `formatLine(index, length, name, value, source)`
- `formatLine(index, length, formats, source)`

**Parameters**

| Parameter | Type     | Description
|-----------|----------|------------
| `index`   | _Number_ | Start index of formatting range.
| `length`  | _Number_ | Length of formatting range.
| `name`    | _String_ | Name of format to apply to text.
| `value`   | _String_ | Value of format to apply to text. A falsy value will remove the format.
| `source`  | _String_ | [Source](/docs/api/#text-change) to be emitted. Defaults to `api`.

**Examples**

{% highlight javascript %}
editor.setText('Hello\nWorld!\n');

editor.formatLine(1, 2, 'align', 'right');   // right aligns the first line
editor.formatLine(4, 4, 'align', 'center');  // center aligns both lines
{% endhighlight %}


### formatText

Formats text in the editor. For line level formats, such as text alignment, target the newline character or use the [`formatLine`](#formatline) helper. See [formats](/docs/formats/) for a list of available formats. The user's selection may not be preserved.

**Methods**

- `formatText(index)`
- `formatText(index, length, name, value)`
- `formatText(index, length, formats)`
- `formatText(index, length, source)`
- `formatText(index, length, name, value, source)`
- `formatText(index, length, formats, source)`

**Parameters**

| Parameter | Type     | Description
|-----------|----------|------------
| `index`   | _Number_ | Start index of formatting range.
| `length`  | _Number_ | Length of formatting range.
| `name`    | _String_ | Name of format to apply to text.
| `value`   | _String_ | Value of format to apply to text. A falsy value will remove the format.
| `source`  | _String_ | [Source](/docs/api/#text-change) to be emitted. Defaults to `api`.

**Examples**

{% highlight javascript %}
editor.setText('Hello\nWorld!\n');

editor.formatText(0, 5, 'bold', true);      // bolds 'hello'

editor.formatText(0, 5, {                   // unbolds 'hello' and set its color to blue
  'bold': false,
  'color': 'rgb(0, 0, 255)'
});

editor.formatText(5, 1, 'align', 'right');  // right aligns the 'hello' line
{% endhighlight %}


### getFormat

Retrieves common formatting of the text in the given range. For a format to be reported, all text within the range must have a value. If there are different values, an array with all values will be reported. If no range is supplied, the user's current selection range is used. May be used to show which formats have been set on the cursor.

**Methods**

- `getFormat()`
- `getFormat(index)`
- `getFormat(index, length)`

**Parameters**

| Parameter | Type     | Description
|-----------|----------|------------
| `index`   | _Number_ | Start index of retrieval.
| `length`  | _Number_ | Length of range.

**Returns**

- _Object_ Formats common to the range.

**Examples**

{% highlight javascript %}
editor.setText('Hello World!');
editor.formatText(0, 2, 'bold', true);
editor.formatText(1, 2, 'italic', true);
editor.getFormat(0, 2);   // { bold: true }
editor.getFormat(1, 1);   // { bold: true, italic: true }

editor.formatText(0, 2, 'color', 'red');
editor.formatText(2, 1, 'color', 'blue');
editor.getFormat(0, 3);   // { color: ['red', 'blue'] }

editor.setSelection(3);
editor.getFormat();       // { italic: true, color: 'blue' }

editor.format('underline', true);
editor.getFormat();       // { italic: true, color: 'blue', underline: 'true' }

editor.formatLine(0, 1, 'align', 'right');
editor.getFormat();       // { italic: true, color: 'blue', underline: 'true', align: 'right' }
{% endhighlight %}


### removeFormat

Removes all formatting and embeds within given range. Line formatting will be removed if the end of the line is included in the range. The user's selection may not be preserved.

**Methods**

- `removeFormat(index, length)`
- `removeFormat(index, length, source)`

**Parameters**

| Parameter | Type     | Description
|-----------|----------|------------
| `index`   | _Number_ | Start index of range.
| `length`  | _Number_ | Length of range.
| `source`  | _String_ | [Source](/docs/api/#text-change) to be emitted. Defaults to `api`.

**Examples**

{% highlight javascript %}
editor.setContents([
  { insert: 'Hello', { bold: true } },
  { insert: '\n', { align: 'center' } },
  { insert: { formula: 'x^2' } },
  { insert: '\n', { align: 'center' } },
  { insert: 'World', { italic: true }},
  { insert: '\n', { align: 'center' } }
]);

editor.removeFormat(3, 7);
// Editor contents are now
// [
//   { insert: 'Hel', { bold: true } },
//   { insert: 'lo\n\nWo' },
//   { insert: 'rld', { italic: true }},
//   { insert: '\n', { align: 'center' } }
// ]

{% endhighlight %}