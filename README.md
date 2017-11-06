# HEXO BLOG'S ENV INSTALL GUIDE

Just a funny blog about some funny things :)

Site's location: http://mystories.vn

## Required packages for run project

1. nodeJS, npm
2. hexo-cli

### Install nodejs and npm

Install via package manager (apt/yum) or download bin from this [site](https://nodejs.org/en/download/) extract it and copy to one of your $PATH directories.

### Install hexo-cli

Install via npm is the easiest way to do.

```
sudo npm install -g hexo-cli
```

Install hexo-cli (or another packages by npm with flag -g) on `docker container` may catch permission error in spite of using `sudo` command. For this error try this bellow solution.

```
sudo chmod 755 /root && mkdir -m 755 -p /root/.npm/_logs
```

## Run project

Run like a normally nodejs project :)

## Start new post

*Note:* you must on `develop` branch when you start writing a new post.

*Step:*

1. Generate new .md file with command:  `hexo new post {your-post-name}`
your post will located on `./source/_post/{your-post-name}.md`
2. Write down your post :)
3. Submit your post and deploy it to my server with command: `./deploy.sh`
4. Submit your post .md file to our git repo (optional). (You should do that :) if not, when I backup post's data on server, your post will be deleted. I will check your pull request and merge it later.)