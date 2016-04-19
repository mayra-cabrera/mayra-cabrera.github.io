The reflog is Git's internal record of every explicit change we've made. Any time, **and I mean any time**, you commit, reset, branch, merge or otherwise update what Git is focused on, Git makes a record of that. That means that **all commits stick around even after we perform a seemingly destructive action like rebasing**. The reflog can be viewed with `git reflog command`

```bash
$ git reflog
1fcc207 HEAD@{0}: commit: Make different path for policy report show view
457371c HEAD@{1}: commit: Fixes policy report logo path
0990467 HEAD@{2}: commit: Restore policy report
f216512 HEAD@{3}: commit: Use PdfCreator inside PolicyReport
```

The reflog is personal and per computer only, so is not transferible. 

Reflog can be focused down to just a specific branch by adding the branch name as an argument, i.e.: `git reflog show update-landing-page`
