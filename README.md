# Run in iTerm

Send a selection or a line to iTerm. This extension just makes available a `run-external.iterm` vscode command. Best used in combination with the [multicommand](https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command) extension and `ipython`. 

Unlike the original repo it's forked from, this version uses `ipython`'s `%paste` magic to make sure formatting is correct. Here's how you might configure multicommand for executing a cell or line and variants that advance to the next cell or line (assumes you're also using the [vscode vim extension](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)).

After installing the multicommand extension, put these in your `settings.json`.

```
    {
      "command": "multiCommand.runCellInIterm",
      "interval": 0,
      "sequence": [
        "python.datascience.selectCellContents",
        "editor.action.clipboardCopyAction",
        "run-external.iterm",
        "extension.vim_escape"
      ]
    },
    {
      "command": "multiCommand.runCellInItermAndAdvance",
      "interval": 10,
      "sequence": [
        "python.datascience.selectCellContents",
        "editor.action.clipboardCopyAction",
        "run-external.iterm",
        "python.datascience.gotoNextCellInFile",
        "extension.vim_escape"
      ]
    },
    {
      "command": "multiCommand.runLineInIterm",
      "interval": 0,
      "sequence": [
        "cursorEnd",
        "cursorHomeSelect",
        "editor.action.clipboardCopyAction",
        "run-external.iterm",
        "cursorEnd"
      ]
    },
    {
      "command": "multiCommand.runLineInItermAndAdvance",
      "interval": 0,
      "sequence": [
        "cursorEnd",
        "cursorHomeSelect",
        "editor.action.clipboardCopyAction",
        "run-external.iterm",
        "cursorDown"
      ]
    }
```

Then you can bind to these new commands in your `keybindings.json.` Here are how mine are set:

```
  {
    "key": "shift+enter",
    "command": "multiCommand.runCellInItermAndAdvance",
    "when": "editorTextFocus && python.datascience.featureenabled && python.datascience.hascodecells && !editorHasSelection"
  },
  {
    "key": "ctrl+enter",
    "command": "multiCommand.runCellInIterm",
    "when": "editorTextFocus && python.datascience.featureenabled && python.datascience.hascodecells && !editorHasSelection"
  },
  {
    "key": "cmd+enter",
    "command": "multiCommand.runLineInIterm",
    "when": "editorTextFocus && python.datascience.featureenabled && python.datascience.hascodecells && !editorHasSelection"
  },
  {
    "key": "cmd+shift+enter",
    "command": "multiCommand.runLineInItermAndAdvance",
    "when": "editorTextFocus && python.datascience.featureenabled && python.datascience.hascodecells && !editorHasSelection"
  },
  {
    "key": "cmd+l cmd+o",
    "command": "-extension.liveServer.goOnline",
    "when": "editorTextFocus"
  },
  {
    "key": "cmd+l cmd+c",
    "command": "-extension.liveServer.goOffline",
    "when": "editorTextFocus"
  },
```

## Installation

1. Install the VSCode extension packaging tool if you don't have it: `npm install -g vsce`
2. Package the extension by running `vsce package` from within this directory
3. Install it `code --install-extension run-external-0.1.2.vsix`

## Usage

It Iterm fire up an `ipython` session. Then create a new `.py` file in VSCode and use your keybindings to have the code execute in Iterm.