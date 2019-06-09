---
layout: post
title: "Starting a Jekyll-based Blog"
date: 2019-06-08 21:00:00
tags: [jekyll, ruby, rbenv]
---
These are the steps I took to get this blog up and running. Since this is a Jekyll-based blog, I could have hosted it @ github.io (i.e., aumisb.github.io). However, the workflow associated with hosting it @ github.io is slower than self-hosting. Specifically, changes will require a `git push` and then take time to propagate to github's servers. As a result, I will self host for now as I build posts and establish a "steady-state". 

---

Steps:

1. Install rbenv. The instructions are well documented [here.](https://github.com/rbenv/rbenv) Make sure to install ruby-build which simplifies the process of installing Ruby versions and verify that rbenv is properly set up using the rbenv-doctor script.

2. List all available Ruby versions available:

    ```bash
    rbenv install -l
    ```
2. Install a desired Ruby version (i.e., 2.6.3)

    ```bash
    rbenv install 2.6.3
    ```

3. To make changes to a specific Ruby version, switch to that version using the following command. Note that this will create a `.ruby-version` in the current directory. This can be deleted if it was created in a non-project directory.

    ```bash
    rbenv local 2.6.3
    ```

4. Install Jekyll and bundler gems:

    ``` bash
    gem install jekyll bundler
    ```

5. For my blog, I chose the [My Stack Problems](https://github.com/agusmakmun/agusmakmun.github.io) theme. To use this theme, fork it, navigate to the corresponding directory, and install any required gems:

    ```bash
    bundle install
    bundle update github-pages 
    ```

6. Serve the blog:

    ```bash
    bundle exec jekyll serve
    ```
    Note that to expose the server to non-localhost addresses add: `--host=0.0.0.0` to the `jekyll serve` command

---
    
To auto-start Jekyll upon start-up. I created and enabled the systemd service below. 

```
[Unit]
Description=Jekyll service
After=syslog.target network-online.target

[Service]
Type=simple
WorkingDirectory=/home/siam/blog
ExecStart=/home/siam/.rbenv/shims/jekyll serve --watch --host=0.0.0.0
ExecStop=/usr/bin/pkill -f jekyll
Restart=always
TimeoutStartSec=60
RestartSec=60
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=jekyll

[Install]
WantedBy=multi-user.target network-online.target
```
