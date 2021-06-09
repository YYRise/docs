---
title: "修改commit"
date: 2021-06-09T10:49:03+08:00
draft: false
---

## 1.修改message

### 修改最近一次提交的
`git commit --amend`

## 2. 修改提交者名字和邮箱

### 改最近一次提交的
`git commit --amend --author="NewAuthor <NewEmail@address.com>"`

### 多个修改
```sh
#!/bin/sh
 
git filter-branch --env-filter '
 
OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"
 
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags

```