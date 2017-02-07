## Handy Git aliases

Less is always more! Git aliases provide shorter commands by collecting options and flags. 

The following is a list of git aliases that I use on a daily basis, the first line is the git alias and the second line is the complete git command. 


```bash
$ git sla  
$ git log --oneline --decorate --graph --all 
```

```bash

$ git glag 
$ git log -E -i --grep
```

```bash

$ git uncommit 
$ git reset --soft HEAD^
```

```bash

$ git unstage 
$ git reset
```

```bash

$ git car
$ git commit --amend --no-edit
```

```bash

$ git aliases 
$ git config --get-regexp alias
```

```bash

$ git upstream 
$ git rev-parse --abbrev-ref --symbolic-full-name @{upstream}
```

```bash

$ git wip
$ git commit -am "WIP"
```


### How to add an alias? 

That's easy you just need to run the following command in your terminal:

```bash
$ git config ---global alias.car = "commit --amend --no-edit"
```

And that's it! You're to ready to use `git car` instead of the verbose option (`git commit --amend --no-edit`), everywhere in your local machine

### How can I see what an alias exactly does?

That's easy too! Just use the `help` command as above

```bash
$ git help car
`git car' is aliased to `commit --amend --no-edit'
```


A very handy little feature huh? :)



