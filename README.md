<img src="https://cdn.rawgit.com/oh-my-fish/oh-my-fish/e4f1c2e0219a17e2c748b824004c8d0b38055c16/docs/logo.svg" align="left" width="144px" height="144px"/>

<h1 align="center">💤 forgit</h1>
<p align="center">
    <em> A plugin for <a href="https://github.com/oh-my-fish/oh-my-fish">Oh My Fish</a> for using git interactively. Powered by <a href="https://github.com/junegunn/fzf">junegunn/fzf</a>. Ported from <a href="https://github.com/wfxr/forgit">wfxr/forgit</a>.</em>
</p>

[![MIT License](https://img.shields.io/badge/license-MIT-007EC7.svg?style=flat-square)](/LICENSE)
[![Fish Shell Version](https://img.shields.io/badge/fish-v3.0.0-007EC7.svg?style=flat-square)](https://fishshell.com)
[![Oh My Fish Framework](https://img.shields.io/badge/Oh%20My%20Fish-Framework-007EC7.svg?style=flat-square)](https://www.github.com/oh-my-fish/oh-my-fish)

This tool is designed to help you use git more efficiently.
It's **lightweight** and **easy to use**.

### 📥 Installation

*Make sure you have [`fzf`](https://github.com/junegunn/fzf) installed.*

```fish
$ omf install forgit
```

### 📝 Features

- **Interactive `git add` selector** (`ga`)

![screenshot](https://raw.githubusercontent.com/wfxr/i/master/forgit-ga.png)

- **Interactive `git log` viewer** (`glo`)

![screenshot](https://raw.githubusercontent.com/wfxr/i/master/forgit-glo.png)

*The log graph can be disabled by option `FORGIT_LOG_GRAPH_ENABLE` (see discuss in [issue #71](https://github.com/wfxr/forgit/issues/71)).*

- **Interactive `.gitignore` generator** (`gi`)

![screenshot](https://raw.githubusercontent.com/wfxr/i/master/forgit-gi.png)

- **Interactive `git diff` viewer** (`gd`)

- **Interactive `git reset HEAD <file>` selector** (`grh`)

- **Interactive `git checkout <file>` selector** (`gcf`)

- **Interactive `git stash` viewer** (`gss`)

- **Interactive `git clean` selector** (`gclean`)

- **Interactive `git cherry-pick` selector** (`gcp`)

### ⌨  Keybinds

| Key                                           | Action                    |
| :-------------------------------------------: | ------------------------- |
| <kbd>Enter</kbd>                              | Confirm                   |
| <kbd>Tab</kbd>                                | Toggle mark and move up   |
| <kbd>Shift</kbd> - <kbd>Tab</kbd>             | Toggle mark and move down |
| <kbd>?</kbd>                                  | Toggle preview window     |
| <kbd>Alt</kbd> - <kbd>W</kbd>                 | Toggle preview wrap       |
| <kbd>Ctrl</kbd> - <kbd>S</kbd>                | Toggle sort               |
| <kbd>Ctrl</kbd> - <kbd>R</kbd>                | Toggle selection          |
| <kbd>Ctrl</kbd> - <kbd>Y</kbd>                | Copy commit hash*         |
| <kbd>Ctrl</kbd> - <kbd>K</kbd> / <kbd>P</kbd> | Selection move up         |
| <kbd>Ctrl</kbd> - <kbd>J</kbd> / <kbd>N</kbd> | Selection move down       |
| <kbd>Alt</kbd> - <kbd>K</kbd> / <kbd>P</kbd>  | Preview move up           |
| <kbd>Alt</kbd> - <kbd>J</kbd> / <kbd>N</kbd>  | Preview move down         |

\* Available when the selection contains a commit hash.
For linux users `FORGIT_COPY_CMD` should be set to make copy work. Example: `FORGIT_COPY_CMD='xclip -selection clipboard'`.

### ⚙  Options

You can change the default aliases by defining these variables below.
(To disable all aliases, Set the `FORGIT_NO_ALIASES` flag.)

``` bash
forgit_log=glo
forgit_diff=gd
forgit_add=ga
forgit_reset_head=grh
forgit_ignore=gi
forgit_restore=gcf
forgit_clean=gclean
forgit_stash_show=gss
forgit_cherry_pick=gcp
```

Forgit will use the default configured pager from git (`core.pager`,
`pager.show`, `pager.diff`) but can be altered with the following environment
variables:

| Use case             | Option                | Fallbacks to                                 |
| ------------         | -------------------   | -------------------------------------------- |
| common pager         | `FORGIT_PAGER`        | `git config core.pager` _or_ `cat`           |
| pager on `git show`  | `FORGIT_SHOW_PAGER`   | `git config pager.show` _or_ `$FORGIT_PAGER` |
| pager on `git diff`  | `FORGIT_DIFF_PAGER`   | `git config pager.diff` _or_ `$FORGIT_PAGER` |
| pager on `gitignore` | `FORGIT_IGNORE_PAGER` | `bat -l gitignore --color always` _or_ `cat` |

You can add default fzf options for `forgit`, including keybinds, layout, etc.
(No need to repeat the options already defined in `FZF_DEFAULT_OPTS`)

``` bash
FORGIT_FZF_DEFAULT_OPTS="
--exact
--border
--cycle
--reverse
--height '80%'
"
```

Customizing fzf options for each command individually is also supported:

| Command  | Option                       |
|----------|------------------------------|
| `ga`     | `FORGIT_ADD_FZF_OPTS`        |
| `glo`    | `FORGIT_LOG_FZF_OPTS`        |
| `gi`     | `FORGIT_IGNORE_FZF_OPTS`     |
| `gd`     | `FORGIT_DIFF_FZF_OPTS`       |
| `grh`    | `FORGIT_RESET_HEAD_FZF_OPTS` |
| `gcf`    | `FORGIT_CHECKOUT_FZF_OPTS`   |
| `gss`    | `FORGIT_STASH_FZF_OPTS`      |
| `gclean` | `FORGIT_CLEAN_FZF_OPTS`      |

Complete loading order of fzf options is:

1. `FZF_DEFAULT_OPTS` (fzf global)
2. `FORGIT_FZF_DEFAULT_OPTS` (forgit global)
3. `FORGIT_CMD_FZF_OPTS` (command specific)

**Example**

- `ctrl-d` to drop the selected stash but do not quit fzf (`gss` specific).
```
FORGIT_STASH_FZF_OPTS='
--bind="ctrl-d:reload(git stash drop $(cut -d: -f1 <<<{}) 1>/dev/null && git stash list)"
'
```

- `ctrl-e` to view the logs in a vim buffer (`glo` specific).
```
FORGIT_LOG_FZF_OPTS='
--bind="ctrl-e:execute(echo {} |grep -Eo [a-f0-9]+ |head -1 |xargs git show |vim -)"
'
```

### 📦 Optional dependencies

- [`diff-so-fancy`](https://github.com/so-fancy/diff-so-fancy) or [`delta`](https://github.com/dandavison/delta): Improve the `git diff` output.

- [`bat`](https://github.com/sharkdp/bat.git): Syntax highlighting for `gitignore`.

- [`emoji-cli`](https://github.com/wfxr/emoji-cli): Emoji support for `git log`.

### 💡 Tips

- Hit `q` to quit from full screen preview any time.
- Commands like `glo`, `gd`, `gcf` and `gclean` accept arguments to restrain the items listed in fzf(eg, `glo develop`, `glo f738479..188a849b -- main.go`, `gclean output/` etc.).
- `gd` supports specifying revision(eg, `gd HEAD~`, `gd v1.0 README.md`).
- Call `gi` with arguments to get the wanted `.gitignore` contents directly(eg, `gi cmake c++`).

### 📃 License

[MIT](https://wfxr.mit-license.org/2017) (c) Wenxuan Zhang, Shawn Anderson

