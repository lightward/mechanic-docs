# Documentation

Individual tasks may be optionally configured with their own **documentation**, formatted with Markdown, providing the user with anything they should know about the task's usage and operation.

Task documentation is shown to the user below the task's [**options**](../options/). Documentation is also shown in the confirmation prompt a user sees when they trigger a task that subscribes to either mechanic/user/trigger or mechanic/user/text.

## Adding documentation

Task documentation may be managed in Advanced mode.

![](<../../../.gitbook/assets/Screen Shot 2022-04-01 at 7.20.38 PM.png>)

In Basic mode, documentation is displayed below the task options.

![](<../../../.gitbook/assets/Screen Shot 2022-04-01 at 7.21.15 PM.png>)

## Markdown

Task documentation may be formatted with Markdown, a common syntax for text formatting.

The following table comes from [markdownguide.org/cheat-sheet](https://www.markdownguide.org/cheat-sheet/), and is presented with no changes, under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) license.

| **Element**                                                                     | **Markdown Syntax**                          |
| ------------------------------------------------------------------------------- | -------------------------------------------- |
| [Heading](https://www.markdownguide.org/basic-syntax/#headings)                 | `# H1 ## H2 ### H3`                          |
| [Bold](https://www.markdownguide.org/basic-syntax/#bold)                        | `**bold text**`                              |
| [Italic](https://www.markdownguide.org/basic-syntax/#italic)                    | `_italicized text_`                          |
| [Blockquote](https://www.markdownguide.org/basic-syntax/#blockquotes-1)         | `> blockquote`                               |
| [Unordered List](https://www.markdownguide.org/basic-syntax/#unordered-lists)   | `- First item - Second item - Third item`    |
| [Ordered List](https://www.markdownguide.org/basic-syntax/#ordered-lists)       | `1. First item 2. Second item 3. Third item` |
| [Code](https://www.markdownguide.org/basic-syntax/#code)                        | `code`                                       |
| [Horizontal Rule](https://www.markdownguide.org/basic-syntax/#horizontal-rules) | `---`                                        |
| [Link](https://www.markdownguide.org/basic-syntax/#links)                       | `[title](https://www.example.com)`           |
| [Image](https://www.markdownguide.org/basic-syntax/#images-1)                   | `![alt text](image.jpg)`                     |

## Keyboard Shortcuts

The Markdown editor in Mechanic includes keyboard shortcuts for common formatting operations. These shortcuts can also be accessed through the **right click menu** in the editor.

<table data-header-hidden><thead><tr><th width="134.640625">Action</th><th width="208.4453125">Shortcut</th><th>Description</th></tr></thead><tbody><tr><td><strong>Action</strong></td><td><strong>Shortcut</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Toggle Bold</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>B</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>B</kbd></p></td><td><p>Adds or removes <strong>Bold</strong> formatting on selected text or current word.</p><pre><code>**text**
</code></pre></td></tr><tr><td><strong>Toggle Italic</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>I</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>I</kbd></p></td><td><p>Adds or removes <em>Italic</em> formatting on selected text or current word.</p><pre><code>_text_
</code></pre></td></tr><tr><td><strong>Toggle Strikethrough</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd></p></td><td><p>Adds or removes <del>Strikethrough</del> on selected text or current word.</p><pre><code>~text~
</code></pre></td></tr><tr><td><strong>Toggle Heading</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>H</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>H</kbd></p></td><td><p>Cycles through <strong>Heading</strong> formats.</p><pre><code><strong>→ H1     → H2      → H3       → Plain Text
</strong>→ # text → ## text → ### text → text 
</code></pre></td></tr><tr><td><strong>Toggle Inline Code</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd></p></td><td><p>Adds or removes <code>Inline Code</code> formatting on selected text or current word.</p><pre><code>`code`
</code></pre></td></tr><tr><td><strong>Toggle Code Block</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd></p></td><td><p>Adds or removes a <code>Code Block</code> on multiline selection or blank line.</p><pre data-full-width="false"><code><strong>```
</strong>code
```
</code></pre></td></tr><tr><td><strong>Toggle List</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>L</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>L</kbd></p></td><td><p>Cycles through <strong>List</strong> formats.</p><pre><code><strong>→ Unordered List → Ordered List → Plain Text
</strong>→ - text         → 1. text      → text
</code></pre></td></tr><tr><td><strong>Toggle Link</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>K</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>K</kbd></p></td><td><p>Adds or removes <strong>Link</strong> formatting.</p><pre><code>[text](url)
</code></pre></td></tr><tr><td><strong>Toggle Blockquote</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd></p></td><td><p>Adds or removes <strong>Blockquote</strong> formatting.</p><pre><code>> text 
</code></pre></td></tr><tr><td><strong>Toggle Image</strong></td><td><p><i class="fa-apple">:apple:</i>: <kbd>⌘</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd> </p><p><br><i class="fa-windows">:windows:</i>: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd></p></td><td><p></p><p>Adds or removes <strong>Image</strong> formatting.</p><pre><code>![](url)
</code></pre></td></tr></tbody></table>

### **Pasting URLs**

When pasting a valid URL, if you have text selected the selected text will automatically be wrapped in Markdown link syntax using the pasted URL. The selected text becomes the link text, and the pasted URL becomes the link destination.

```
→ [selected text](https://pastedurl.com)
```

