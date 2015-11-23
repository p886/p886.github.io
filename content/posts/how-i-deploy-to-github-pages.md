+++
Description = "My workflow for deploying to GitHub pages"
date = "2015-11-23T12:25:34+01:00"
title = "How I deploy to GitHub Pages"

+++

### How does Github pages work? (skip if you know this)

Many static pages generators will generate a single `public` as output. This folders contains the entire generated site. On GitHub you can create a repository with the special name `<your GitHub username>.github.io`, in my case `p886.github.io`. Doing so will create a webpage at the address `http://github_user_name.github.io`.

### The problem with static page generators and GitHub pages
The tricky part is that the `master` branch of that repository should contain the site (the content of the aforementioned `public` folder), while on the other hand you probably want to keep the source files of that page (in my case of collection of markdown files) in the same repository.

### The solution
My [p886.github.io repository](https://github.com/p886/p886.github.io) has two branches: `master` and `source`. `master` contains the generated HTML, which is copied over on each deployment from the `source` branches' `public` folder. How do I go about copying the HTML files from one branch to another? I found that the easiest solution is to have the project cloned locally two times. One clone contains only the master branch, another one only the source branch. When creating a post I add the markdown file in the `source` clone. After commiting the new post I run the static page generator to create the `public` directory. Subsequently I copy the contents of the public directory to the `master` clone. A new commit in the `master` checkout is created which contains the site. This commit is then pushed to the remote repository to have the new content online.

In reality I automated most of this. After I created the commit containing the new post in the `source` checkout I run this bash script:

<script src="https://gist.github.com/p886/955d2b48aa2718639e70.js"></script>

### Summary
The key is two have two branches in the repository: `master` and `source`. Locally the repository is cloned twice: once with the `master` and again with the `source` branch.
