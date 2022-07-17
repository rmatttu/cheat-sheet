# Visual Studio Code

## Screen mode

* Zen mode: `Ctrl + K, Z`
* Zoom up: `Ctrl + -`
* Zoom down: `Ctrl + Shift + -`

## vscode vim

* easymotion: `<Leader><Leader>, w`
* sneak

```
s<search word 1><search word 2>
: (next)
, (prev)
```

## Snippet

```jsonc
"Template": {
  "prefix": "template",
  // transform "/.*/" to ""
  // /path/to/current-dir -> current-dir
  // not work "${TM_DIRECTORY:/.*/::} ",
  // not work "${TM_DIRECTORY:/.*/::g} ",
  // not work "${TM_DIRECTORY//.*///g} ",
  // not work "${TM_DIRECTORY:\/.*\/::g} ",
  // not work "${TM_DIRECTORY/\/.*\///g} ",
  // not work "${TM_DIRECTORY:\\/.*\\/::g} ",
  "body": [
    "# ${TM_DIRECTORY/\\/(.*)\\///}", // Parent dir name
    "",
    "$1",
    "",
    "## Usage",
    "",
    "## Requirements",
    "",
    "## Installation",
    "",
    "## License",
    "",
    "## Author",
    "",
    "## References"
  ],
}
```

## 参考

* [Snippets in Visual Studio Code](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
* [How to get the base directory in visual studio code snippet? - Stack Overflow](https://stackoverflow.com/questions/48366014/how-to-get-the-base-directory-in-visual-studio-code-snippet)
