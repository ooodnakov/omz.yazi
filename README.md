# omz.yazi

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Show your [Oh My Zsh](https://ohmyz.sh) prompt inside [Yazi](https://yazi-rs.github.io).

`omz.yazi` replaces Yazi's default header with the prompt your shell actually
renders — theme colors, git decorations, current directory and all — and keeps
it in sync as you change directories or tabs. It is the Oh My Zsh counterpart of
[omp.yazi](https://github.com/saumyajyoti/omp.yazi) for oh-my-posh.

```
➜  src  main ✔
```

## Requirements

- Yazi `25.5.28` or newer
- zsh with Oh My Zsh installed and working interactively

## Installation

With Yazi's package manager:

```sh
ya pkg add ooodnakov/omz.yazi
```

Or manually:

```sh
git clone https://github.com/ooodnakov/omz.yazi.git ~/.config/yazi/plugins/omz.yazi
```

Then add this to `~/.config/yazi/init.lua`:

```lua
require("omz"):setup()
```

Restart Yazi.

## Configuration

```lua
require("omz"):setup {
	-- Interpreter used to render the prompt.
	-- Defaults to $SHELL when it is zsh, otherwise "zsh" from PATH.
	shell = "/bin/zsh",
}
```

## How it works

On startup and on every `cd`/`tab` event the plugin runs an interactive zsh in
the new directory and asks it for its prompt:

```sh
zsh -i -c 'print -P -- "$PROMPT"'
```

Only the text between the plugin's private markers is used, so banners printed
during shell startup (version managers, framework notices) never leak into the
header. The command runs as a Yazi job, so a slow shell startup never blocks the
file manager; the header updates as soon as the prompt is ready.

If a shell starts without applying its theme, the plugin sources
`$ZSH/themes/${ZSH_THEME}.zsh-theme` once before printing the prompt.

## Notes

- Only `$PROMPT` is shown. `$RPROMPT`, right-side decorations, and second prompt
  lines are not rendered because the Yazi header is a single line.
- The prompt is rendered for the directory Yazi is in, not for individual file
  operations performed inside Yazi.
- Prompt frameworks that assemble their output outside `$PROMPT` may show a
  reduced line when their initializer cannot run in a non-interactive shell.

## License

MIT — see [LICENSE](LICENSE).
