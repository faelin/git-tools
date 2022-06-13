# git-tools
a collection of utilities to simplify git usage

## Installation

1. Clone this repo into your home directory (or anywhere else)

2. Include this file in your global .gitconfig file:

```gitconfig
# ~/.gitconfig

[include]
    path = PATH/TO/.git_tools/.gitconfig
```

or run the following commands to automatically add the git-tools:
```sh
#!/bin/sh

export GIT_TOOLS_PATH=#PATH/TO/.git_tools/.gitconfig
GIT_CONFIG="$HOME/.gitconfig"

grep -qx '^\[include\]$' "$GIT_CONFIG" 2>/dev/null || echo '\n[include]' >> "$GIT_CONFIG";
grep -F "path = ${GIT_TOOLS_PATH}" "$GIT_CONFIG" 2>/dev/null || sed -i '' -e "s/^[include]$/[include]\n    path = ${GIT_TOOLS_PATH}/" "$GIT_CONFIG";
```

3. Add the following line to your shell profile (.bashrc, .zshrc, etc):
```bash
# ~/.bashrc

[[ -z "$GIT_TOOLS_PATH" ]] && export GIT_TOOLS_PATH="$(dirname "$(git config --show-origin alias.xtool | sed 's/^file:\\([^      ]*\\).*/\\1/')")"
```

## Usage 
### `$> git alias <name>`
  print the indicated git alias

### `$> git aliases`
  prints all of the aliases listed in the git config

### `$> git clear-changes`
  reset all changes back to the state of HEAD (most recent commit)

### `$> git ignore [--global] [path(s)]`
  adds the specified path to the local .gitignore file
  if no path is specified, opens .gitignore in the default editor
```
 --global
   acts on the ~/.gitignore_global file instead
```

### `$> git make-like <ref>`
  pulls all files from the indicated ref (commit, branch, etc) into your current working tree
  and merges non-conflicting changes, without overwriting your working tree state

### `$> git nearby`
  intended to list all git projects in the parent directory of the current project

### `$> git-pluck [options] [branch] [commit|files...]`

**THIS IS A WORK IN PROGRESS AND MAY NOT BE FULLY FUNCTIONAL YET**

  cherry-pick the indicated commit or files from the specified branch and add them to the working tree
```
 -i,--interactive
    enters an interactive search utility to visualize changes
      - if an entire commit is selected, all changes will be shown in diff format
      - after selecting a commit, specific filestates at that commit can be selected
        to apply only the changes for that specific file
```

#### `$> git-pluck --clean <branch>`
 as above, but with a clean cache.

#### `$> git-pluck --reset`
 clear the git-pluck cache

### `$> git purge [filepath(s)]`

**WARNING: THIS IS VERY DANGEROUS AND CANNOT BE UNDONE!**

  remove the indicated file(s) from the working tree and scrub them entirely from the git history

 - all references to the removed files are scrubbed from the repo history
 - any commit which soley references the targeted file(s) is removed from the repo history
 - there is no way to undo this

### `$> git rollback [commit-ish] [n]`
  undo the last (n) commits while preserving the working tree (defaults to undo the most recent commit)

### `$> git root`
  prints the root directory 

### `$> git send ["message"]`
  commit all tracked files with the indicated message and push to git

### `$> git status-diff [<options>] [<commit>] [--] [<path>…]`
  see colorized diff for changes in current working tree

### `$> git touch <filename>`
  begin tracking the indicated files (creates an empty file if it does not exist)

### `$> git tree [--modified]`
  print the directory tree from the root of the current git context
```
 -m, --modified
    only show files that have been modified since the last commit
```

### `$> git unstage <files>`
  unstage the indicates file(s) in the working

### `$> git untrack <files>`
  remove the indicates files from the working tree, thus no longer tracking them via git

### `$> git xtool [--open|-o] <tool>`
  execute the indicated tool, or open the tool file
