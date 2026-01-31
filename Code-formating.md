Friday only reservation node

https://help.obsidian.md/advanced-syntax

```
sinteractive --reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=2 --qos=normal
```
Node for any other time
```
ainteractive --
```
Working Directory
```
/scratch/alpine/c832916267@colostate.edu
```

# This is a heading 1
## This is a heading 2
### This is a heading 3
#### This is a heading 4
##### This is a heading 5
###### This is a heading 6

| Style                  | Syntax                 | Example                                  | Output                                 |
| ---------------------- | ---------------------- | ---------------------------------------- | -------------------------------------- |
| Bold                   | `** **` or `__ __`     | `**Bold text**`                          | **Bold text**                          |
| Italic                 | `* *` or `_ _`         | `*Italic text*`                          | _Italic text_                          |
| Strikethrough          | `~~ ~~`                | `~~Striked out text~~`                   | ~~Striked out text~~                   |
| Highlight              | `== ==`                | `==Highlighted text==`                   | ==Highlighted text==                   |
| Bold and nested italic | `** **` and `_ _`      | `**Bold text and _nested italic_ text**` | **Bold text and _nested italic_ text** |
| Bold and italic        | `*** ***` or `___ ___` | `***Bold and italic text***`             | **_Bold and italic text_**             |
Formatting can be forced to display in plain text by adding a backslash `\` in front of it.

**This line will not be bold**

```markdown
\*\*This line will not be bold\*\*
```

*_This line will be italic and show the asterisks_*

```markdown
\**This line will be italic and show the asterisks*\*
```

If you want to link to an external URL, you can create an inline link by surrounding the link text in brackets (`[ ]`), and then the URL in parentheses (`( )`).

```md
[Obsidian Help](https://help.obsidian.md)
```

You can also create external links to files in other vaults, by linking to an [Obsidian URI](https://help.obsidian.md/Extending+Obsidian/Obsidian+URI).

```md
[Note](obsidian://open?vault=MainVault&file=Note.md)
```
You can add images with external URLs, by adding a `!` symbol before an [external link](https://help.obsidian.md/syntax#External%20links).

```md
![Engelbart](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
```
You can change the image dimensions, by adding `|640x480` to the link destination, where 640 is the width and 480 is the height.

```md
![Engelbart|100x145](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
```

If you only specify the width, the image scales according to its original aspect ratio. For example:

```md
![Engelbart|100](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
```

Tip

## Quotes 

You can quote text by adding a `>` symbols before the text.

```md
> Human beings face ever more complex and urgent problems, and their effectiveness

\- Doug Engelbart, 1961
```

> Human beings face ever more complex and urgent problems, and their effectiveness 

- Doug Engelbart, 1961

# Lists
- You can create an unordered list by adding a `-`, `*`, or `+` before the text.

```md
- First list item
- Second list item
- Third list item
```

- First list item
- Second list item
- Third list item

To create an ordered list, start each line with a number followed by a `.` or `)` symbol.

```md
1. First list item
2. Second list item
3. Third list item
```

1. First list item
2. Second list item
3. Third list item

```md
1) First list item
2) Second list item
3) Third list item
```

1. First list item
2. Second list item
3. Third list item

You can use `Shift+Enter` to insert a [line break](https://help.obsidian.md/syntax#Line%20breaks) within an ordered list without altering the numbering.

```md
1. First list item
   
2. Second list item
3. Third list item
   
4. Fourth list item
5. Fifth list item
6. Sixth list ite
```

### Nesting lists 

You can nest any type of list—ordered, unordered, or task lists—under any other type of list.

To create a nested list, indent one or more list items. You can mix list types within a nested structure:

```md
1. First list item
   2. Ordered nested list item
3. Second list item
   - Unordered nested list item
```

1. First list item
    1. Ordered nested list item
2. Second list item
    - Unordered nested list item

Similarly, you can create a nested task list by indenting one or more list items:

```md
- [ ] Task item 1
	- [ ] Subtask 1
- [ ] Task item 2
	- [ ] Subtask 1
```

- [ ] Task item 1
    - [ ] Subtask 1
- [ ] Task item 2
    - [ ] Subtask 1

Use `Tab` or `Shift+Tab` to indent or unindent selected list items to easily organize them.

## Escaping Markdown Syntax 

In some cases, you may need to display special characters in Markdown, such as `*`, `_`, or `#`, without triggering their formatting. To display these characters literally, place a backslash (`\`) before them.

Common characters to escape

- Asterisk: `\*`
- Underscore: `\_`
- Hashtag: `\#`
- Backtick: `` \` ``
- Pipe (used in tables): `\|`
- Tilde: `\~`

```md
\*This text will not be italicized\*.
```

*This text will not be italicized*.

When working with numbered lists, you may need to escape the period after the number to prevent automatic list formatting. Place the backslash (`\`) before the period, **not** before the number.

```md
1\. This won't be a list item.
```

1. This won't be a list item.