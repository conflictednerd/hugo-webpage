---
title: "Setting Up Hugo with GitHub Pages"
date: 2022-07-21T22:29:29+04:30

categories:
- Web

description: A simple guide on creating a webpage just like this!
tags:
- programming
- web

keywords: [Programming, Web, Hugo, GitHub Pages]

ShowTOC: true
include_toc: true
draft: false
---

This is a straight-forward guide on setting up a simple and beautiful website using Hugo + PaperMod theme and GitHub pages. This is basically what I did to set up this very website!

There are two steps. We first install Hugo and create our website locally. Then we use GitHub pages to publish our website on the Internet. Once this is done, changing our website is as simple as a `git commit`! (Admittedly, a git commit can get very complicated at times!🙂)

Let's begin!

## Installing Hugo Locally

1. Follow the first two steps [here](https://gohugo.io/getting-started/quick-start/). This will install Hugo on your local machine and create a website.

   1. **Note:** When creating the site, use `hugo new site <name of site> -f yml` to create a `yml` config file.

2. From step 3, follow [this guide](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation) to add the **PaperMod theme**.

   1. To update the theme, go to `themes/PaperMod` directory and run `git pull`.

3. **Serving Locally:** We have now successfully installed Hugo along with a theme. To see how things look, open the command line at the project location and run `hugo server`.

4. To **add some content** run `hugo new <category>/<file>.md`. This will create a new post in the `content` folder. You can then edit it.

   1. Change `draft: true` to `draft: false` in the header section of your posts to have them published.
   2. We almost always use `category = posts`. Although you can add other categories as well.

5. **Adding math and Latex support:** Navigate to the `layouts` directory. There create a new directory named `partials` and inside it create an html file named `extend_head.html`. Put the following code inside it: 

   ```html
   <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css">
   <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js"></script>
   
   <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js" onload="renderMathInElement(document.body);"></script>
   
   <script>
       document.addEventListener("DOMContentLoaded", function() {
           renderMathInElement(document.body, {
               delimiters: [
                   {left: "$$", right: "$$", display: true},
                   {left: "$", right: "$", display: false}
               ]
           });
       });
   </script>
   ```

6. **Config file:** Inside the main directory there is a `config.yml` file that stores the configuration of the website. For a decent starting point, you can copy my configurations. With a few tweaks, it should work fine for you.

   1. Put your images in the `static/` directory

7. To set up the **archives and the search page**, you should add an `archives.md` and a `search.md` file to your `contents` directory. They should contain the following codes:

   ```markdown
   ---
   title: "Archive"
   layout: "archives"
   # url: "/archives"
   summary: "archives"
   ---
   ```

   ```markdown
   ---
   title: "Search" # in any language you want
   layout: "search" # is necessary
   # url: "/archive"
   # description: "Description for Search"
   summary: "search"
   placeholder: "placeholder text in search input box"
   ---
   ```

8. For adding **additional features**, such as comments, check out [this](https://github.com/adityatelange/hugo-PaperMod/wiki/Features) page.

## GitHub Pages

1. In your GitHub account, create a repo **with the same name as your username**.

2. In the `config.yml` file of your website, change `baseURL` to `https://YourGitHubUsername.github.io/`.

3. From the command line, run the `hugo` command in your main directory. This will create a `public` directory which is what we will publish.

4. Go to the newly created `public` directory. There, we will create a repo and connect it with the remote repo that we just created:

   ```bash
   git init
   git add .
   git remote add origin https://github.com/username/username.github.io.git
   git commit -m "first commit"
   git push origin master
   ```

5. Once the content of `public` directory was pushed to your GitHub, go to your repo's page on GitHub. Go to `Settings -> Pages`. Under the source section, select your master branch and the `/(root)` directory and create your website.

6. Done! You can see your website at `YourGitHubUsername.github.io`. (This may take a few minutes)

7. **Issue:** After doing all this, my website didn't display correctly (it was raw html without formatting). I had to disable fingerprinting by adding the following code to the config file:

   ```yaml
   params:
       assets:
           disableFingerprinting: true
   ```

   This solved the issue.

## Updating Your Webpage

Each time you want to update your page you must do so in your local machine, run the `hugo` command and commit the changes that are made to the `public` directory. Any changes that you make will automatically take effect after you commit them.

So let's say we want to add a new post. We will

1. `hugo new posts/MyNewPost.md`

2. Go to the `content/posts` directory and edit `MyNewPost.md`. (You can also change the meta-data at the top of this file to add tags, etc. Take a look at the source code for this post!)

3. ```bash
   hugo
   cd public
   git add .
   git commit -m "Added a new post"
   git push origin master
   ```
