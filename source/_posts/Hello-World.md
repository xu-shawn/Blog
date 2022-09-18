---
title: Hello World
date: 2022-09-05 00:52:31
tags:
- Network
- GitHub Page
  categories:
- Network
---
# Hello World!
If you see this article, that means this blog run successfully!

## Installation

- For MacOS User

```bash
$ brew install hexo
```

- Using NPM to install

```bash
$ npm install hexo -g	
```

## Configing the blog

### Initialize the blog

```bash
$ hexo init blog # 'blog can be replaced as any name'
```

```bash
$ cd blog
```

### Installing the theme

```bash
$ git clone https://github.com/fluid-dev/hexo-theme-fluid ./themes/fluid
```

```yml
# ./_config.yml
theme: fluid
```

### Customize the information showed

```yml
# ./_config.fluid.yml
navbar:
	# your custom title
  blog_title: 'Branden Xia'
index:
  slogan:
 		# your custom slogan
    text: "Blog of Branden Xia"
about:
	# put your avatar file into ./source/img/avatar.jpeg
  avatar: /img/avatar.jpeg
  # your name
  name: Branden Xia
  # your custom introduction
  intro: "A high school student"
  icons:
  	# Github
    - { class: "iconfont icon-github-fill", link: "https://github.com/BrandenXia", tip: "GitHub" }
    # Steam
    - { class: "iconfont icon-steam", link: "https://steamcommunity.com/id/brandenxia", tip: "Steam"}
    # Email
    - { class: "iconfont icon-mail", link: "mailto:xxtbranden@outlook.com", tip: "Email"}
```

```yml
# ./_config.yml
title: Blog of Branden Xia
author: Branden Xia
url: https://BrandenXia.github.io
```

### Try this blog locally

```bash
$ hexo g -d
$ hexo s
```

Then, visit `localhost: 4000` to view the blog.

### Create new page

```bash
# use your custom name instead
$ hexo new post HelloWorld
# regenerate whole blog
$ hexo g -d
```

Edit `./source/_posts/HelloWorld.md`

```markdown
---
title: HelloWorld
date: 2022-09-05 00:52:31
tags:
- Network
categories:
- Network
---
Write your article here
```

### Deploy blog to GitHub page

```yml
deploy:
	type: git
	# replace username with your own username, remember to create repository in GitHub first
	repo: https://github.com/username/username.github.io.git
	branch: main
	# your personal access token
	token: token
```
