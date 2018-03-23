---
layout: post
title: Solution to "argument list too long" error
---

Sometimes when there are millions of files in a directory, we cannot rm, cp or mv all the files. 
We will get the error like  

```
sdhuang32@sd-google-ubuntu:~$ rm ./*
/bin/rm: cannot execute [Argument list too long]
```

According to [this post](https://www.cyberciti.biz/faq/argument-list-too-long-error-solution/)
we can see the limitation of length of command line arguments:  
```
sdhuang32@sd-google-ubuntu:~$ getconf ARG_MAX  
2097152  
```
  
### So, how can we solve this problem?

* find + -exec
```
find . -type f -maxdepth 1 -exec rm -f {} \;
```

* echo + xargs 
```
echo ./* | xargs rm -f
```

### What if we still cannot delete the files because our partition/volume runs out of space?

* truncate the files first!
```
find . -type f -maxdepth 1 -exec truncate -s 0 {} \;
find . -type f -maxdepth 1 -exec rm -f {} \;
```
