# git 查看指令帮助信息

执行以下指令会提示在浏览器中查看对应指令的相关信息

```bash
git help <verb>
git <verb> --help
```

若想在命令行窗口查看帮助信息，使用以下命令

```bash
git <verb> -h
```

# git commit

Record changes to the repository.

## Description

Create a new commit containing the current contents of the index and the given log message describing the changes. The new commit is a direct child of HEAD, usually the tip of the current branch, and the branch is updated to point to it.

The content to be committed can be specified in several ways:

- by using `git add` to incrementally "add" changes to the index before using the *commit* command (Note: even modified files must be "added");
- by using `git rm` to remove files from the working tree and the index, again before using the *commit* command;
- by listing files as arguments to the *commit* command (without `--interactive` or `--patch` switch), in which case the *commit* will ignore changes staged in the index, and instead record the current content of the listed files (which must already be known to Git); (只commit已列出的文件，即使其他文件也经过修改)
- by using the `-a` switch with the *commit* command to automatically "add" changes from all known files (i.e. all files that are already listed in the index) and to automatically "rm" files in the index that have been removed from the working tree, and then perform the actual commit;
- by using the `--interactive` or `--patch` switches with the *commit* command to decide one by one which files or hunks should be part of the commit in addition to contents in the index, before finalizing the operation. 

If you make a commit and then find a mistake immediately after that, you can recover from it with `git reset`.

## Options

`-a`

`--all`

Tell the command to automatically stage files that have been modified and deleted, but new files you have not told Git about are not affected. (可以跳过执行`git add` 命令，可与`-m`结合为`-am` option)

`-C <commit>`

`--reuse-message=<commit>`

Take an existing commit object, and reuse the log message and the authorship information (including the timestamp) when creating the commit.

`-c <commit>`

`--reedit-message=<commit>`

Like `-C`, but with `-c` the editor is invoked, so that the user can further edit the commit message.

`--author=<author>`

Override the commit author. 

`--date=<date>`

Override the author date used in the commit.

`-m <msg>`

`--message=<msg>`

Use the given `<msg>` as the commit message. If multiple `-m` options are given, their values are concatenated as <u>separate paragraphs</u>. (比如添加两个`-m`选项，在commit 日志中，将显示两行message)

`--amend`

Replace the tip of the current branch by creating a new commit. The recorded tree is prepared as usual , and the message from the original commit is used as the starting point, instead of an empty message, when no other message is specified from the command line via options such as `-m`, `-F`, `-c`, etc. The new commit has the same parents and author as the current one. (修订最近的一次commit)

# `.gitignore`

## Description

A `gitignore` file specifies intentionally untracked files that Git should ignore. <u>Files already tracked by Git are not affected</u>.

Each line in a `gitignore` file specifies a pattern. When deciding whether to ignore a path, Git normally checks `gitignore` patterns from multiple sources (e.g. command line, `.gitignore` file), with the following order of precedence, from highest to lowest (within one level of precedence, the last matching pattern decides the outcome):

- Patterns read from the command line for those commands that support them. (命令行优先级最高)
- Patterns read from a `.gitignore` file in the same directory as the path, or in any parent directory (up to the top-level of the working tree), with patterns in the higher level files being overridden by those in lower level files down to the directory containing the file. These patterns match relative to the location of the `.gitignore` file. A project normally includes such `.gitignore` files in its repository, containing patterns for files generated as part of the project build.  (其次`.gitignore`文件)
- Patterns read from `$GIT_DIR/info/exclude`. (`.git`目录下`/info/exclue`文件，路径相对于`.git`所在目录)
- Patterns read from the file specified by the configuration variable `core.excludesFile`.

Which file to place a pattern in depends on how the pattern is meant to be used.

- Patterns which should be version-controlled and distributed to other repositories via clone (i.e., files that all developers will want to ignore) should go into a `.gitignore` file.
- <u>Patterns which are specific to a particular repository but which do not need to be shared with other related repositories (e.g., auxiliary files that live inside the repository but are specific to one user’s workflow) should go into the `$GIT_DIR/info/exclude` file.</u>
- Patterns which a user wants Git to ignore in all situations (e.g., backup or temporary files generated by the user’s editor of choice) generally go into a file specified by `core.excludesFile` in the user’s `~/.gitconfig`. Its default value is `$XDG_CONFIG_HOME/git/ignore`. If `$XDG_CONFIG_HOME` is either not set or empty, `$HOME/.config/git/ignore` is used instead.

## Pattern format

- A blank line matches no files, so it can serve as a separator for readability.
- A line starting with # serves as a comment. <u>Put a backslash ("`\`") in front of the first hash for patterns that begin with a hash.</u>
- Trailing spaces are ignored unless they are quoted with backslash ("`\`").
- An optional prefix "`!`" which negates the pattern; any matching file excluded by a previous pattern will become included again (先前模式排除的任何匹配文件将再次包含在内). It is not possible to re-include a file if a parent directory of that file is excluded. Git doesn’t list excluded directories for performance reasons, so any patterns on contained files have no effect, no matter where they are defined. Put a backslash ("`\`") in front of the first "`!`" for patterns that begin with a literal "`!`", for example, "`\!important!.txt`".
- The slash */* is used as the directory separator. Separators may occur at the beginning, middle or end of the `.gitignore` search pattern.
- <u>If there is a separator at the beginning or middle (or both) of the pattern, then the pattern is relative to the directory level of the particular `.gitignore` file itself.</u> <u>Otherwise the pattern may also match at any level below the `.gitignore` level.</u>
- <u>If there is a separator at the end of the pattern then the pattern will only match directories, otherwise the pattern can match both files and directories.</u>
- For example, a pattern `doc/frotz/` matches `doc/frotz` directory, but not `a/doc/frotz` directory; however `frotz/` matches `frotz` and `a/frotz` that is a directory (all paths are relative from the `.gitignore` file).
- An asterisk "`*`" matches anything except a slash. The character "`?`" matches any one character except "`/`". The range notation, e.g. `[a-zA-Z]`, can be used to match one of the characters in a range. 

Two consecutive asterisks ("`**`") in patterns matched against full pathname may have special meaning:

- A leading "`**`" followed by a slash means match in all directories. For example, "`**/foo`" matches file or directory "`foo`" anywhere, the same as pattern "`foo`". "`**/foo/bar`" matches file or directory "`bar`" anywhere that is directly under directory "`foo`".
- A trailing "`/**`" matches everything inside. For example, "`abc/**`" matches all files inside directory "`abc`", relative to the location of the `.gitignore` file, with infinite depth.
- A slash followed by two consecutive asterisks then a slash matches zero or more directories. For example, "`a/**/b`" matches "`a/b`", "`a/x/b`", "`a/x/y/b`" and so on.
- Other consecutive asterisks are considered regular asterisks and will match according to the previous rules.

## Examples

- The pattern `hello.*` matches any file or directory whose name begins with `hello.`. If one wants to restrict this only to the directory and not in its subdirectories, one can prepend the pattern with a slash, i.e. `/hello.*`; the pattern now matches `hello.txt`, `hello.c` but not `a/hello.java`.
- The pattern `foo/` will match a directory `foo` and paths underneath it, but will not match a regular file or a symbolic link `foo`.
- The pattern `doc/frotz` and `/doc/frotz` have the same effect in any `.gitignore` file. In other words, a leading slash is not relevant if there is already a middle slash in the pattern.
- The pattern "`foo/*`", matches "`foo/test.json`" (a regular file), "`foo/bar`" (a directory), but it does not match "`foo/bar/hello.c`" (a regular file), as the asterisk in the pattern does not match "`bar/hello.c`" which has a slash in it.

