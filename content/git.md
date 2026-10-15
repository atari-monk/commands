## Git

### Single commit

- adds current changes to last commit
- this is only for one person repos !

- use when all edits in changes state (none staged):

```sh
project=""
path="/home/atari-monk/atari-monk/project/$project"

git -C "$path" add . && \
git -C "$path" commit --amend --no-edit && \
git -C "$path" push --force
```

- use for staged edits:

```sh
project=""
path="/home/atari-monk/atari-monk/project/$project"

git -C "$path" commit --amend --no-edit && \
git -C "$path" push --force
```
