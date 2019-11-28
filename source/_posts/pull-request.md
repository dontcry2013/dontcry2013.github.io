---
title: Pull Request
date: 2019-11-25 23:31:22
categories: Git #文章文类
tags: [GitHub] #文章标签，多于一项时用这种格式
---
# Pull Request is a Feature of GitHub
Pull request can be used in GitHub and platforms like GitLab and GitBucks.
It is not supported in your server if you make a repository by using git.

<!--more-->
# git am
```
$ curl -L http://github.com/cbeust/testng/pull/17.patch | git am
```
git am command is used to merge a patch into your repository.
```
$ curl https://github.com/sclasen/jcommander/commit/bd770141029f49bcfa2e0d6e6e6282b531e69179.patch 
```
You can also merge a specific commit. In github, navigate to the pull request tab, and you will find similar url and adding '.patch' as suffix to get the patch.

# Add PR Submitter as Remote  
```
# create the remote repository
$ git remote add nullin git://github.com/nullin/testng.git

# pull all codes from the remote
$ git fetch nullin

# merge to local branch
$ git merge kneath/error-page

# push to your repository
$ git push origin master
```

# How to Create a PR With Specific Commits
You cannot really do anything on the GitHub UI to remove any commits.
You are suppose to create a branch with the latest changes and cherry-pick the ones that you want. And then compare that branch with the repository that you would like to contribute.
The differences will only be the ones that you cherry-picked. Then, you are done. 
```
$ git remote add upstream https://github.com/upstream_github_username/upstream_github_repo_name.git
$ git checkout -b new-branch-name upstream/master
$ git cherry-pick a51afa6
$ git cherry-pick 07f39f7
$ git push -u origin new-branch-name
```
Or if you have a an existing branch which tracking the remote repo, for example, named main(upstream branch would be like [main/master])
```
# In branch main
$ git checkout -b new-branch-name
# And then do the above cherry-pick steps
```
After this, GitHub repo page will display a button to let you to create a PR directly.
When the owner of the forked repo concluded your PR, you can view the owners closed PR page, in there, you can delete the branch created for the PR solely.
Note: This move only delete your upstream branch located in GitHub, you have to delete it manually in local.
```
# sync and remove the stale branch
$ git fetch --all --prune
$ git branch -d new-branch-name
```
When development keeps going on, you can sync with the forked repo and create another PR. To do this, checkout main branch which tracking with [main/master], and pull from main/master
```
$ git checkout main
$ git pull main master
$ git checkout -b another-branch-name
$ git checkout master
$ git merge main
$ git checkout another-branch-name
$ git cherry-pick a51afae
$ git cherry-pick v313cva
$ git push -u origin another-branch-name
```