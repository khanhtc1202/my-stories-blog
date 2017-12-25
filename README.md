# BLOG'S CONTRIBUTION GUIDE

This is a funny blog about some funny things :)

Site's location: https://mystories.vn

## About this guide

1. [Contribute a new post](#contribute-a-new-post)
2. [Run project on local for testing](#run-project-on-local-for-testing)

## Contribute a new post

**Note:** you must on `develop` branch when you start writing a new post.

**Steps:**

If you don't have `hexo-cli` on your local pc, don't worry! Do like below.

1. Generate new .md file with command: `./new_post.sh {your-post-name}`
your post will be located on `./source/_post/{your-post-name}.md`
2. Write down your post :)
3. Checkout a new branch and push into our git repo. After that, create pull request to `develop` branch. After my approvation, your post will automatically deployed to my server. Thanks for your contribution :).

If you installed `hexo-cli` package. (Install guide go [here](#install-hexo-cli))

1. Generate new .md file with command:  `hexo new post {your-post-name}`
your post will be located on `./source/_post/{your-post-name}.md`
2. Write down your post :)
3. Submit your post and deploy it to my server with command: `./deploy.sh` (only done if you are our team members).
4. Submit your post .md file to our git repo (optional). (You should do that :) if not, when I backup post's data on server, your post will be deleted. `develop` branch was protected so you must checkout to new branch and create pull request to `develop` branch. I will check your pull request and merge it later.)

## Run project on local for testing

Required packages for run project on local

1. nodeJS, npm
2. hexo-cli

### Install nodejs and npm

Install via package manager (apt/yum) or download bin from this [site](https://nodejs.org/en/download/) extract it and copy to one of your $PATH directories.

### Install hexo-cli

Install via npm is the easiest way to do.

```
sudo npm install -g hexo-cli
```

Install `hexo-cli` (or another packages by npm with flag -g) on `docker container` may catch permission error in spite of using `sudo` command. For this error try this below solution.

```
sudo chmod 755 /root && mkdir -m 755 -p /root/.npm/_logs
```

## Run project

Run like a normally nodejs project :) that mean you must run `npm install` on the first time you run this project.

Start hexo server

```
hexo server -p 3000
```

Local blog will be available at `http://localhost:3000/`
