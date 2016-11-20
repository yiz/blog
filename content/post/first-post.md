+++
date = "2016-11-20T14:03:43-08:00"
title = "first post"
draft = false

+++

hugo new site yiz
cd yiz
hugo new post/first-post.md
hugo server --buildDrafts
cd themes
git clone https://github.com/alanorth/hugo-theme-bootstrap4-blog.git
cd ..
hugo server --theme=hugo-theme-bootstrap4-blog --buildDrafts

Edit config.toml

hugo undraft content/post/first-post.md

hugo

cd public
git init
git remote add origin git@github.com:yiz/yiz.github.io.git
git add --all
git commit -m 'initial commit'
git push --set-upstream origin master

cd ..
git init
git remote add origin git@github.com:yiz/blog.git
rm -rf public
git submodule add git@github.com:yiz/yiz.github.io.git public
git submodule add https://github.com/alanorth/hugo-theme-bootstrap4-blog.git
