---
title: Using git to upload your blogs
date: 2018-12-09
tags: [blog_conf]
categories: blog
---

- Links worth of considerring
> https://blog.csdn.net/shile/article/details/78714189

- Git init in the directory
> `git init`
> Initialized empty Git repository in /media/pold/Work/Leopold.Sun/Blog/leopold-sun/.git/

- Configure SSH-key of github
> `ssh-keygen -t rsa -C your-email`
> put the id_rsa.pub in your github ssh settings

- Git pull the repository of github
> `git clone git@github.com:Leopold-Sun/Leopold-Sun.github.io.git`
> 

- Connect local repository with remote one
> `git remote add origin git@github.com:Leopold-Sun/Leopold-Sun.github.io.git` *"origin" presents remote repository*

- Commit to github
> `git add .` *将要提交的文件信息（包括有修改过和新建的文件）添加到索引库*
> `git commit -m "message"` *根据索引库的内容进行文件的提交*
> `git push origin master`

- Creat one branch and commit to it
> `git branch pold-blog`
> `git checkout pold-blog` *switch to anothe branch*
> add programs that you want to commit to github.
> `git add .`
> `git commit -m "message"`
> here, if you haven't connect your local repository with the remote, you may use `git remote add origin url` that the "url" should be you target github repository.
> `git push origin pold-blog`

- check infos of branch
> check local branches
> `git branch`
> check remote branches
> `git branch -r`
> switch to local branch
> `git checkout ${local branch}`

- Merge branches
> from remote branch to local branch
> `git pull origin ${remote branch}:${local branch}` *git pull <远程主机名> <远程分支名>:<本地分支名>*
> or 
> `git pull origin ${remote branch}` *we could delete the local branch name to merge local active branch with remote branch*
> Or we could fetch first and merge following
> `git fetch origin`
> `git merge origin:${remote branch}$` *local branch is hided* 
> push local branch to remote branch
> `git push origin ${remote branch}:${local branch}`

- Make a backup branch for your hexo configuration
> `git init` *initialize your local repo*
> `git add ${files you wanted to backup}`
> `git commit -m "message"`
> `git branch ${new branch}`
> `git checkout ${new branch}`
> `git remote add origin ${repo url}`
> `git push origin ${new branch}` *this would creat a new branch on your remote branch*

- Blog migrated to a new env
> you should already have installed nodejs+git+hexo
> `git clone -b ${backup branch} git@github.com:user/user.github.io.git  //将Github中hexo分支clone到本地`
> `cd user.github.io`
> `npm install`
> `git init`
> `git remote add origin ${repo url}`
> `git pull origin ${backup branch on origin repo}`

- Backup and deployment of new blogs
> `git pull origin ${local branch}` *本地与远端融合*
> `hexo new post "new post name"` 
> `git add source`
> `git commit -m "message"`
> `git push origin ${the backup branch}` *backup*
> `hexo d -g` *deployment*
