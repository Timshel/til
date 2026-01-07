# Git

## Alias

Some git alias

```  
        lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold b>
        s = status
        d = !"git diff-index --quiet HEAD -- || clear; git diff --patch-with-stat"
        difc = diff --cached
``` 

## View the diff of just one Commit

You just to do the diff between the preceding commit `hash^` and the commit.

```
git diff 572ecd1^ 572ecd1
```

## Git stage patch edit

When staging chunk using `git add -p` if after splitting `s` the result is not splitted enough just directly edit the patch using `e`.

## Create a patch with latest commit

```bash
>git format-patch -1 HEAD
```

## Apply patch

```bash
>git apply --verbose --directory=fenix patch.patch
```

If there is conflict use three-ways merge:

```bash
>git am --ignore-whitespace --ignore-space-change -3 patch.patch
```

## Commit Hash

```bash
>git rev-parse HEAD
>git rev-parse --short HEAD
```

## Multiline commit messages

Useful to create release messages on tag.

```bash
git tag 2023-09-05 -a
```

## Push multiple tags

```bash
git push codeberg tag "v2025*"
```

## Delete multiple tags

```bash
git push codeberg -d $(git tag -l "v2.*")
```
