# showcase.spoon

This is a Hammerspoon module (*Spoon*) used to create showcase GIFs for project READMEs.

## Usage

Showcases are scripted with a simple text file whose path you pass into the `RunShowcase` function. This file is read and executed line by line. What happens depends on the start of the line. Lines starting with:

- `type: ` will type the rest of the line's content with the keyboard (supports all ASCII characters on a US keyboard layout)
- `key: ` allow you to have the showcase press more complex keybindings including modifiers, e.g. `key: ctrl-cmd space`
- `delay: ` will delay execution of the next line for the specified number of seconds
- `execute: ` will execute the rest of the line as Lua in Hammerspoon, allowing you to do just about anything
- `#` are ignored as comments

and empty lines are skipped.

## Install

- install this repository to `~/.hammerspoon/spoons/showcase.spoon`
- run `hs.loadSpoon("showcase")` whenever you want to run a showcase – there is (probably) no reason to always load this in your config

## Examples

The showcase you see above is scripted in [`example.txt`](example.txt). Some other projects that use this to create their showcase are:

- [Vivify](https://github.com/jannis-baum/Vivify) and its [showcase script](https://github.com/jannis-baum/Vivify/blob/assets/showcase-script.txt)
- [Jupyviv](https://github.com/jannis-baum/Jupyviv) and its [showcase script](https://github.com/jannis-baum/Jupyviv/blob/assets/showcase-script.txt)
