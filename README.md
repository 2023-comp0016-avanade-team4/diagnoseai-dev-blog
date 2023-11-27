# DiagnoseAI Development Blog

This repository contains both the source code for the DiagnoseAI
Development Blog, and also the GitHub pages deployment environment for
the site.

The site is generated from the Hugo site generator.

## Pre-requisites

You will need to install [Hugo](https://gohugo.io/installation/), and
[NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

## Running

Run the `hugo` server by doing this:

``` shell
hugo
```

If you want to see drafts, do this:

``` shell
hugo -D
```

To generate a date to populate the front matter, run the following
command:

``` shell
date -I'seconds'
```
