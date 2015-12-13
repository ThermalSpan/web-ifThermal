---
layout: post
title: Website Setup
tags:
    -jekyll
    -bash
    -bluehost
    -git
---

Website Setup
=============

Building this website presented a nice sequence of challenges for me. First, I still need to gain experience with the *html* and *css* combo. Secondly, I had to learn about stic site generators. Lastly I had to figure out a suitable setup for version control and pushing updates to the bluehost server I use. 

Webdesign presents me with several challenges. The most prominent is that i'm not a designer. To overcome this challenge I decided to stick with simplicity. So, aside from the masking gimmick in the header I chose to implement a simple black and white theme. All the content lives in a single column. I tried to keep the extra fluff to a minimum. Expect more formatting and design updates as I gather feedback.

After I had a rough [prototype](https://github.com/ThermalSpan/web-experiments) that showcased the look I was going for it was time to learn about static site generation. I chose to make a static site becuase I found the other option to be overwhelming. The operation of a program that uses some markup to produce a complete directory of html files makes good sense to me. In addition it seems like a good idea to get familiar with jekyll since that was the system of choice for github. Couldn't be simpler to get it running, just follow the directions on their [website](http://jekyllrb.com).

The next step was to automate the process of updating the web server whenever I made a change to the site. I will save the details of how I arrived at the following solution, I will merely say that I could not get jekyll to work on the server and leave it at that. The major point is that I now have two repositories for this website,one for Jekyll and the other for the site build. These are [web-ifThermal](https://github.com/ThermalSpan/web-ifThermal) and [web-ifThermal-site](https://github.com/ThermalSpan/web-ifThermal-site) respectivley. The webserver has a clone of the web-ifThermal-site repository. I created a soft-link from the repository to the public_html folder.

    rm -r -f ~/public_html
    ln -T ~/projects/web-ifThermal-site ~/public_html

Since I build the website locally, I wrote a pre-commit hook for the Jekyll repository that takes care of building and updating the site repository on both Gihub and the server. Here's what it does:

1) It builds the site using:

    jekyll build -d ~/projects/web-ifThermal-site

2) It then pushs the new site build to github.
    
    NOW=$(date +"%H:%M-%b-%d-%Y")
    git add -A
    git commit -m "Site build - ${NOW}"
    git push origin master

3) Lastly, It uses [sshpass](http://sshpass.sourceforge.net) and ssh to do a git pull on the server's site repository. 

    source $HOME/ifthermal.cfg
    cmd="cd $HOME/projects/web-ifThermal-site; git pull;  exit;"
    sshpass -p$password ssh ${sshuser}@${domain} $cmd

Now, in step three you may have noticed my use of source to collect sensitive information from a configuration file. It seems to be a debatable issue on best practices for calling secure information from a script. However, this file will be relativley safe on my machine so I'll leave it that. If this solution rubs you the wrong way and you have a better solution PLEASE contact me. To be clear, the config file looks like this:
    #!/bin/bash
    sshuser=user
    domain=example.com
    password=12345

So now my workflow is fairly simple. I make changes to the site, update layouts, add a post, etc. When I'm ready I commit those changes, which will also update my site. So there it is, my current website setup. 

Here is a gist of the pre-commit script. Funny story, I was having trouble with gist and redid this one around ten times over the course of a couple minutes. ANNNND I got my account hidden for a while. Note: to use a the hook it must be in the the .git/hooks directory, and cannot have a file extension. 

<script src="https://gist.github.com/ThermalSpan/2d82e99cca87be958b08.js"></script>


