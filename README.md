# showcase.spoon

This is a Hammerspoon module (*Spoon*) used to create showcase GIFs for project READMEs to make this as streamlined and simple as possible. See below for a demo of what you could do with this.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/jannis-baum/showcase.spoon/refs/heads/assets/dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/jannis-baum/showcase.spoon/refs/heads/assets/light.gif">
  <img alt="Showcase" src="https://raw.githubusercontent.com/jannis-baum/showcase.spoon/refs/heads/assets/dark.gif">
</picture>

## Usage

Showcases are scripted with a simple text file whose path you pass into the `RunShowcase` function. This file is read and executed line by line. What happens depends on the start of the line. Lines starting with:

- `type: ` will type the rest of the line's content with the keyboard (supports all ASCII characters on a US keyboard layout)
- `key: ` allow you to have the showcase press more complex keybindings including modifiers, e.g. `key: ctrl-cmd space`
- `delay: ` will delay execution of the next line for the specified number of seconds
- `execute: ` will execute the rest of the line as Lua in Hammerspoon, allowing you to do just about anything
- `#` are ignored as comments

and empty lines are skipped.

## Hints

This is some advice for how to record, handle and add your GIF to your README. Some credit for this goes to [this gist](https://gist.github.com/joncardasis/e6494afd538a400722545163eb2e1fa5) and [this superuser answer](https://superuser.com/a/556031).

### Recording

- Hit `cmd-shift 5`, select "Record Selected Portion", select the part of your screen you want to record, and hit "Record" to start the recording
- After having loaded the showcase Spoon, run `RunShowcase("/path/to/your/script.txt")` in the Hammerspoon console.
- The console will close automatically and your script will run
- Once it is done, click the button in the menubar or hit `cmd-ctrl escape` to stop the recording

> [!tip]
> You can also start and stop the recording automatically with your showcase script after having configured the selected portion:
>
> ```plain
> # start recording
> key: cmd-shift 5
> delay: 0.3
> key: return
> delay: 0.3
>
> # ... your script
>
> # stop recording
> key: cmd-ctrl escape
> ```
>
> Note that the delays are needed.

### Making a GIF

To turn your recording into a 1000 pixel wide GIF (enough for a GitHub README) with `ffmpeg`, run

```sh
ffmpeg -i recording.mov \
    -vf "fps=30,scale=1000:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" \
    -loop 0 recording.gif
```

Note that you may want to reduce the frame rate to e.g. 15 in case your file is too big in the end. I would suggest that a reasonable GIF should be at most 5MB.

Then you can use `gifsicle` to optimize the file size of the GIF

```sh
gifsicle --optimize=3 --threads=$(sysctl -n hw.ncpu) recording.gif -o showcase.gif
```

### Adding the GIF to your README

Adding one or multiple GIFs straight to your code repository will make its total file size a lot bigger than it needs to be. An alternative approach is to create an orphan `assets` branch where you keep your bigger files so that the main branch (and other code branches) can be kept slim. This is slightly adapted from [this gist](https://gist.github.com/joncardasis/e6494afd538a400722545163eb2e1fa5), for more details you can read that original.

1. Clone a fresh copy of your repo to avoid breaking anything
2. Run `git checkout --orphan assets` to create a new orphan branch with no commits
3. Run `git rm -rf .` to remove all files from the git tree
4. Add and commit your GIF, `git add showcase.gif; git commit -m "docs: add showcase"`
5. Push it, `git push origin assets`

This `assests` branch is also a great place to store your showcase script and have it source-controlled and safe without polluting your main repo!

You can now link to your stored GIF in your `README.md` (or anywhere) using its GitHub raw URL. This the preferred way of linking it since it will keep the `README` in tact in case it is moved somewhere else or viewed outside of git context. The URL will look something like this:

```plain
https://raw.githubusercontent.com/user/repo/refs/heads/assets/showcase.gif
```

### GitHub supports theming images

You can [add images specific to the viewer's dark/light mode](https://github.blog/developer-skills/github/how-to-make-your-images-in-markdown-on-github-adjust-for-dark-mode-and-light-mode/) to GitHub. Since you are already using a script to do all the work, it's a great opportunity to make your README look nice for everyone.

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/user/repo/refs/heads/assets/showcase-dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/user/repo/refs/heads/assets/showcase-light.gif">
  <img alt="Showcase" src="https://raw.githubusercontent.com/user/repo/refs/heads/assets/showcase-dark.gif">
</picture>
```

## Install

- install this repository to `~/.hammerspoon/spoons/showcase.spoon`
- run `hs.loadSpoon("showcase")` whenever you want to run a showcase – there is (probably) no reason to always load this in your config

## Examples

The showcase you see above is scripted in [`example.txt`](example.txt). Some other projects that use this to create their showcase are:

- [Vivify](https://github.com/jannis-baum/Vivify) and its [showcase script](https://github.com/jannis-baum/Vivify/blob/assets/showcase-script.txt)
- [Jupyviv](https://github.com/jannis-baum/Jupyviv) and its [showcase script](https://github.com/jannis-baum/Jupyviv/blob/assets/showcase-script.txt)
