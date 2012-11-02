---
title: About this blog
date: '2012-10-31'
description: Some details about how is this blog made
categories:
---

I felt the need to start blogging for a while already, and I think I'm finally pretty close to do so.

This blog is [static](http://github.com/div/blog) and it is hosted on the github pages. All the generation is done by [ruhoh](http://ruhoh.com/ "Ruhoh") - a cool and simple alternative to jekyll and octopress. The only thing you'll need to add to ruhoh is a simple rake task for deploy on github pages.


```ruby
desc "deploy build directory to github pages"
task :deploy do
  cd "compiled/#{REPO}" do
    system "git init ."
    system "git add ."
    system "git add -u"
    message = "Updated at #{Time.now.utc}"
    system "git commit -m \"#{message}\""
    system "git push git@github.com:#{USER}/#{REPO} master:gh-pages --force"
    system "rm -rf .git"
  end
end
```