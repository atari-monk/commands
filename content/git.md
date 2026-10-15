## Git

### Single commit

Adds current changes to last commit

```sh
project=""
path="/home/atari-monk/atari-monk/project/$project"

git -C "$path" add . && \
git -C "$path" commit --amend --no-edit && \
git -C "$path" push --force
```
