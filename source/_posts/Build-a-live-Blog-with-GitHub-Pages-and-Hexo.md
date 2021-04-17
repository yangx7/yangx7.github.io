---
title: Build a live Blog with GitHub Pages and Hexo
date: 2021-04-17 00:32:55
tags:
---

Welcome to my blog! This is my very first post, in this post, I would like to introduce how I build this blog with GitHub Pages and Hexo step by step. Let's get started! 

### Step 1. Creating a GitHub Pages site
Why choose GitHub Pages? [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) is a static site hosting service that you can use to host a website about yourself, your organization, or your project directly from a GitHub repository. It is very easy to use this service. Note the keyword "static", it means GiHub Pages only support websites to show static content instead of interacting with website users such as updating shopping cart like what they do on Walmart.com. Based on this fact, GitHub Pages is particularly suitable for hosting a blog website, since in most cases blog websites are just displaying static text and images. 

In the future I may add new features such as a comment system using some third-party services, at that time, our blog website will be able to do simple interaction with blog viewers, but this is beyond the scope of this blog. Let's focus on creating our GitHub Pages Site.

Here is the detailed instruction of [how to creating a GitHub Pages Site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site), following the instruction we can finally get our website like this:<img src="https://i.loli.net/2021/04/17/ycYBp2Clom5NuPD.png" width="60%">

### Step 2. Install Node.js and npm
Before we can really touch Hexo, we need to install Node.js and npm firstly. Node.js is the runtime environment for Hexo and npm is used to install hexo and many other modules we may use in the future. 

Here is the [link](https://nodejs.org/en/) to download Node.js, I always prefer the LTS (Long Term Support) version because it is primarily aimed at enterprise use and hence always more stable than the current Node.js version. After download, just follow the installer and finally we can get our Node.js installed. To verify installation, we can use command 
```
  node -v
``` 
to check the Node.js version, the result should be look similar like this: <img src="https://i.loli.net/2021/04/17/jhnYiEbFIoSMasB.png" width="20%">

When you download Node.js, you automatically get [npm](https://www.npmjs.com/get-npm) installed on your computer. To check if npm installed correctly, use command 
```
  npm -v
``` 
to check the npm version, the result should be something like this: <img src="https://i.loli.net/2021/04/17/KPyh7xqu6piTzER.png" width="20%">

It is worth noting that [npx](https://www.npmjs.com/package/npx) is also installed with npm, it is the npm package runner which is pre-bundled with npm. We will use npx soon.

### Step 3. Install and Setup Hexo
What is Hexo? Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other markup languages) and Hexo generates static files with a beautiful theme in seconds. What's more, Hexo helps us quickly deploy posts to github, so everyone can view your posts in no time.

Hexo official website provides a detailed instruction on [how to install Hexo](https://hexo.io/docs/), to install Hexo, run the command 
```
  npm install -g hexo-cli
```
Once installed, for Windows user like me, I can use ```npx hexo <command>``` to run Hexo. Note here we use npx as mentioned above, without beginning with npx, the command may not be recognized by the command prompt.
