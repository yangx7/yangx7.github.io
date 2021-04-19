---
title: Build a live Blog with GitHub Pages and Hexo
date: 2021-04-17 00:32:55
tags:
---

Welcome to my blog! This is my very first post, in this post, I would like to introduce how I build this blog with GitHub Pages and Hexo step by step. Let's get started! 

### Step 1. Creating a GitHub Pages site
Why choose GitHub Pages? [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) is a static site hosting service that you can use to host a website about yourself, your organization, or your project directly from a GitHub repository. It is very fast and easy to use this service. Note the keyword "static", it means GiHub Pages only support websites to show static content instead of interacting with website users such as updating shopping cart and checking out like what we do on Walmart.com. Based on this fact, GitHub Pages is particularly suitable for hosting a blog website, since in most cases blog websites just display static text and images. 

In the future I may add new features such as a comment system using some third-party services, at that time, our blog website will be able to do some simple interaction with blog viewers, which is beyond the scope of this blog. For now, let's focus on creating our GitHub Pages Site.

Here is the detailed instruction of [how to creating a GitHub Pages Site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site), the document gives very clear explanation for each step so I won't go over it again, just following the instruction we can finally get our website like this empty page just showing the repo name: <img src="https://i.loli.net/2021/04/17/ycYBp2Clom5NuPD.png" width="60%">
<span style="color:red">Please note that for individual users like me, the repo created must be named ```<user>.github.io```, here the user name must match the owner name, and the GitHub Pages url we finally get should be ```https://<user>.github.io```.</span>

The GitHub Pages website will show posts we put on the main branch of our repo, so next several steps we will focus on creating our local posts and finally deploy those posts to the main branch, at that time, our blog will go live.

### Step 2. Install Node.js and npm
Before we touch Hexo, we need to install Node.js and npm. Node.js is the runtime environment for Hexo and npm is used to install Hexo and many other modules we may use in the future. 

Here I build the blog under Windows OS, so just choose the [Windows version of Node.js](https://nodejs.org/en/) and download, I always prefer the LTS (Long Term Support) version because it is primarily aimed at enterprise use and hence always more stable than the current Node.js version. After download, just follow the installer and get Node.js installed. To verify installation, we can use command:
```
  node -v
``` 
to check Node.js version, the result should be similar like this: <img src="https://i.loli.net/2021/04/17/jhnYiEbFIoSMasB.png" width="18%">

When you download Node.js, you automatically get [npm](https://www.npmjs.com/get-npm) installed on your computer. To check if npm is installed correctly, use command:
```
  npm -v
``` 
to check the npm version, the result should be like this: <img src="https://i.loli.net/2021/04/17/KPyh7xqu6piTzER.png" width="18%">

It is worth noting that [npx](https://www.npmjs.com/package/npx) is also installed with npm, it is the npm package runner which is pre-bundled with npm. We will use npx soon.

### Step 3. Install Hexo
What is Hexo? [Hexo](https://hexo.io/docs/) is a fast, simple and powerful blog framework. You write posts in Markdown (or other markup languages) and Hexo generates static files with a beautiful theme in seconds. What's more, Hexo can help quickly deploy posts to github, so everyone can view your blog in no time.

Hexo official website provides a detailed instruction on [how to install Hexo](https://hexo.io/docs/). You may notice that Hexo requires us to install Git as well, at this point, we can just skip this step since Git can be tricky for beginners and for now we do not need it. To install Hexo, run the command:
```
  npm install -g hexo-cli
```
Once installed, for Windows user like me, I can use ```npx hexo <command>``` to run Hexo commands. <span style="color:red">Note here we use npx as mentioned above, without beginning with npx, the command may not be recognized by the Windows Command Prompt or PowerShell</span>. For example, to check if Hexo is installed successfully, run the command:
```
  npx hexo -v
```
we will get the result like this: <img src="https://i.loli.net/2021/04/18/3FuU9f8SLgts5y6.png" >

### Step 4. Initialize Hexo in the target folder
Alright, after Hexo is installed, we can [initialize a Hexo project in the target folder](https://hexo.io/docs/setup). For Windows user, go to the drive you want to put your project and run the command:
```
  npx hexo init <folder>
```
Here I initialize a Hexo project in the "blog" folder in drive D, "blog" folder is created after I run the above command. <img src="https://i.loli.net/2021/04/18/eaGRdH7VNprTZi9.png" >

Next I need to go inside the new created folder "blog" ```cd blog``` and install all dependencies Hexo project required. Just run below command:
```
  npm install
```

At this point, we successfully initialize a Hexo project. Hexo official website lists lots of useful [commands](https://hexo.io/docs/commands), but for now let's just start a local server to check if we can see any content locally. Run the command:
```
  npx hexo server
```
to start a local server. If everything goes well, then by default going to http://localhost:4000/ on our browser, we should see the initial Hello-World blog initialized by Hexo. <img src="https://i.loli.net/2021/04/18/oFTU1yeWKBYNhxA.png" width="60%">

### Step 5. Install Git
Now we have a local Hexo project, we can start putting down some interesting posts in the project and see posts through local server. But our goal is to build a live blog able to be visited by everyone, so we need to install Git to deploy our project to GiHub, where Gihub Pages starts working.

Here is the link to download [64-bit version of Git for Windows](https://git-scm.com/download/win), we can always check the version to verify installation, run the command:
```
  git --version
```
we should see git version displayed: <img src="https://i.loli.net/2021/04/18/IreybLCioRz2pc9.png" >

After download Git, check this instruction [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) to setup basic Git configuration. The most important settings for the first time setup is identity, that are user name and email address. 

### Step 6. Hexo One-Command Deployment
At this point, we can start thinking about deploying our project to GitHub. Hexo provides two github deployment strategies, here I use the [One-Command Deployment](https://hexo.io/docs/one-command-deployment) method, which is fast and easy to use. First we need to install [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git), run the following command:
```
  npm install hexo-deployer-git --save
```
Then we need to edit _config.yml in our project to tell Hexo where to deploy the project, use any code editor to edit the file, my setting shows below:
```
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/yangx7/yangx7.github.io.git
  branch: main
```
On the instruction given by Hexo, we can also add message setting, here I prefer the default format for this field which records the timestamp: "Site updated: YYYY-MM-DD HH:mm:ss". Therefor we can just ignore the message field. 

To finish the deployment, just run these commands following the order:
```
  npx hexo clean
  npx hexo deploy
```
Or we can put them in one line:
```
  npx hexo clean; npx hexo deploy
```
If these commands execute successfully, the deployment is done. We can check our main branch to see the change: <img src="https://i.loli.net/2021/04/18/rNwsJzKxgAbQPOt.png" width="60%">

Go to the link we get from GitHub Pages in Step 1, we finally get our blog live: <img src="https://i.loli.net/2021/04/18/RHjFOpzqirghal2.png" width="60%">

Now we have been able to successfully write blog locally and then have everyone see on the internet. Next time if we want to write a new blog, just write down the blog in Markdown, open the local Hexo server to verify the blog content, and finally use the one-command deployment strategy to deploy the blog. Having this basic flow, our blog website is starting to get on track. Of course we can modify the theme of our blog website, add more third-party features and so on, which would make the blog more fascinating, we will explore in the future.

### Step 7. [Optional] Push source code to GitHub
One thing that concerns me is that one-command deployment only deploys blog content to GitHub, but what I want is at the same time pushing my whole project source code to GitHub, so next time if I change to another computer, I can directly clone the repo from GitHub and quickly start wroking on the same project. This is an optional step, if you do not worry about this concern like me, you can just skip this step.

To achieve this goal, we need to integrate Git just like any other GitHub projects. First thing we need to do is to make our project a git project, just run the command inside our blog folder:
```
  git init
```
we should see the result like this: <img src="https://i.loli.net/2021/04/18/X2WKqt4SvUf56yI.png" >

Next, we need to setup the target GitHub repo, run commnads one by one:
```
  git remote -v
  git remote add origin <your_repo_url>
  git remote -v
```
After the first ```git remote -v```, we see nothing showing since at this point on our local we do not set any GitHub repo as our destination yet. Then the second command we setup a repo and name it origin. The repo url can be obtained directly from our GitHub repo the one we want to store the project source code. After we add the origin repo, run ```git remote -v``` again, we should see the target repo now: <img src="https://i.loli.net/2021/04/18/TLX7ZFx2WynwMQd.png" >

To push our source code from local to the GiHub repo, we need to specify a local branch, that is because a GitHub repo can have multiple branches and each branch can maitain different codes, we need to choose a branch to put our code. Let's specify our source code in one branch called develop. Run commands below:
```
  git branch
  git checkout -b develop
  git branch
```
The command ```git branch``` is used to list all branches we have locally and it also shows which branch we are currently at. After the first ```git branch``` command, we see nothing there since there is not any branch created yet. We then create a branch named develop and switch to this branch with the command ```git checkout -b develop```. <span style="color:red">Please note this is actually a combination of two more basic commands: ```git branch <branch_name>``` is used to create a new branch, ```git checkout <branch_name>``` is used to switch to the target branch. The shortcut command is ```git checkout -b <branch_name>```.</span> 

But even after we create the develop branch, if we run ```git branch``` again, we still cannot see any branch listed there, because actually at this point our source code does not belong to the develop branch yet and our develop branch contains nothing. So next step, we are going to commit our source code to develop branch. Perhaps following commands are the most frequently used over git users:
```
  git status
  git add .
  git commit -m "some message"
```
The first command is to check which files are changed compared to the last commit, after run this command, we should see the similar: <img src="https://i.loli.net/2021/04/18/45yIpTaLKecnWtE.png" width="50%">
Since there is not commit yet, so every file we create in the project is regarded as new added or namely changed. Then we run ```git add .``` to add all these files in the git staging area, check this post [Saving changes](https://www.atlassian.com/git/tutorials/saving-changes#:~:text=The%20git%20add%20command%20adds,until%20you%20run%20git%20commit%20.) to see more details about this command. Finally we commit these changes by running ```git commit -m "some message"```, we can custom the message along with the commit.

After commit done, develop branch now contains our source codes. Run command:
```
  git branch
```
we should see develop branch listed there: <img src="https://i.loli.net/2021/04/18/GQJv26siktqgnEw.png" >

The last thing we need to do is to push this develop branch which contains all our source code to our remote GitHub repo. Run command:
```
  git push origin develop
```
The Command Prompt or the PowerShell should show something like this: <img src="https://i.loli.net/2021/04/18/MZFhUOND43TSW2j.png" width="50%">

At this time, we successfully push our source code to GitHub, check our GitHub repo, we can see a new branch develop is created automatically which contains our source code: <img src="https://i.loli.net/2021/04/18/LRkjGcUgmhdl1CA.png" width="60%">

So next time after we write some new post, we can use one-command deployment to put our post on the main branch, and at the same time, we can commit the change to git and push the change of source codes to our develop branch. These two processes do not interfere with each other. 

A main difference between develop and main branch is that develop branch contains our original Markdown blog files while the main branch has those blogs in html format which are generated after Hexo deployment.

### Summary
We have come to the end of this blog, in this blog, I introduce my method to build this live blog with the help of GitHub Pages service and Hexo framework. This is a good start, we figure out the whole flow on how to initialize a blog project, deploy the blog to GitHub and finally let everyone see our blog through GitHub Pages service. Bear this flow in mind, we are able to write a blog and make it go live quickly with confidence. In the future, I will explore methods to change my blog theme and integrate other fancy third-party features to make my blog website better!