---
title: Travis Practice
date: 2019-12-06 23:46:03
categories: [CI, Basic, Tools]
tags: [GitHub, Travis]
---
Coding is just part of the software development, and more time usually spent on building and testing.

In order to improve the efficiency of software development, automated tools for building and testing are constantly emerging. [Travis CI](https://travis-ci.org) is one of these tools with the largest [market share].
![travis](/img/travis-logo.png "Travis CI")

<!--more-->

# What is CI?

With the first glance of the name, people often ask what is CI? Well, CI stands for continuous integration.

Continuous integration refers to automatically running builds and tests whenever the code changes, and feeding back the results of the operation. After ensuring that it meet expectations.

The benefit of continuous integration is that you can see the results of each small change in the code, and continually accumulate small changes instead of merging a large piece of code at the end of the development cycle.

Travis CI provides Continuous Integration (CI). It is bound to the project on Github, and whenever there is new code, it will automatically grab it. Then, provide a runtime environment, perform tests, complete the build, and deploy to the server.

# Preparation

Travis CI have two sites，**`travis-ci.org`** targeted at open source project, Open source project hosted on Github can use it freely. **`travis-ci.com`** is for commercial projects, new user can build up to 100 times freely.

In order to try it on freely, you have to meet the following conditions:
```
- Have a GitHub account
- There is an open source project under this account
- The project has runnable code
- The project also contains build or test scripts
```

# Try It
- Add [Travis CI] to your account.
- Go to [Applications settings], configure Travis CI to have access to the repo.

  ![TravisCI in GitHub][TravisCI-GitHub]
- You’ll be redirected to Travis page.
- On a new tab, generate a [new token] with repo scopes. Note down the token value.
  
  ![Token in GitHub][Token-GitHub]
- On the Travis page, go to your repo’s setting. Under **`Environment Variables`**, put **`GH_TOKEN`** as name and paste the token onto value. Click Add to save it.
- Add .travis.yml file to your repo, just like below.
``` yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS or lts/*
cache: npm
branches:
  only:
    - hexo # build hexo branch only
script:
  - hexo generate # generate static files
after_script:
  - cd ./public
  - git init
  - git config user.name "your name"
  - git config user.email "your email"
  - git add .
  - git commit -m "Update blog content by Travis CI"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
```
> **`GH_REF`** is your repo url, mine is https://github.com/dontcry2013/dontcry2013.github.io.git

- Once Travis CI finish the deployment, the generated pages can be found in the master branch of your repository

> This script actually created for my blog to auto deploy which triggered by push commit, I made a branch **`hexo`** to hold the markdown raw files, that is also the default branch, and **`master`** servers the generated html files.

# Encrypted Information

If you are not assured that confidential information is clearly on the Travis website, you can use the encryption provided by Travis.

First, install Ruby's package `travis`.
``` ruby
$ gem install travis
```

You can then use `travis encrypt` commands to encrypt the information.

In the root directory of the project, execute the following command.
``` ruby
$ travis encrypt SOMEVAR=secretvalue
```

After execution, the following information will be output on the screen.
``` ruby
secure: ".... encrypted data ...."
```

Now you can add this line `.travis.yml`.
``` yaml
env:
  global:
    - secure: ".... encrypted data ...."
```
Then, the environment variable can be used in the script `$SOMEVAR`, and Travis will automatically decrypt it at runtime.

The `--add` parameter will automatically write the encrypted text to the `.travis.yml`, eliminating the need to modify the env fields.
```
$ travis encrypt SOMEVAR=secretvalue --add
```
See the [official documentation] for details .










 Refer: 
 - http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html
 - https://hexo.io/docs/github-pages
 - https://xirikm.net/2019/826-2


[market share]: https://github.com/blog/2463-github-welcomes-all-ci-tools
[Travis CI]: https://github.com/marketplace/travis-ci
[Applications settings]: https://github.com/settings/installations
[new token]: https://github.com/settings/tokens
[official documentation]: https://docs.travis-ci.com/user/encryption-keys/

[TravisCI-GitHub]: /img/travis-github.png "Travis in GitHub"
[Token-GitHub]: /img/travis-github-token.png "Token in GitHub"