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
