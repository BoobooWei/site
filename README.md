# 魏亚萍的个人网站

<!-- Markdown snippet -->
[![Build Status](https://travis-ci.org/hexojs/site.svg?branch=master)](https://travis-ci.org/hexojs/site)

网站源码修改自 `The website for Hexo`.

## Getting started

Install dependencies:

``` bash
$ git clone xxx.git
$ cd mysite
$ npm install
```

Generate:

``` bash
$ hexo generate
```

Run server:

``` bash
$ hexo server
```

## Commands

```bash 
dir=/oracle/oracle-12c/student/;ll | grep md| awk '{print $9}' | awk -F '.md' '{print "@"$1"@: %dir%"$1".html"}' | sed "s/@/'/g;s@%dir%@${dir}@g"
ll | grep md| awk '{print $9}' | awk -F '.md' '{print "@"$1"@: @"$1"@"}' | sed "s/@/'/g"
```
