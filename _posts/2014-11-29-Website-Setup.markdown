---
layout: post
title: Website Setup
---

Website Setup
=============

Building this website presented a nice sequence of challenges for me. First, I have very little experience with the *html* and *css* combo. Secondly, I had to learn about stic site generators. Lastly I had to figure out a suitable setup for version control and pushing updates to bluehost server I use. 

Webdesign presents me wih several challenges. The most prominent is that i'm not a designer. To overcome this challenge I decided to stick with simplicity. So, aside from the masking gimmick in the header I chose to implement a simple black and white theme. All the content lives in a single column. I tried to keep the extra fluff to a minimum. 


After I had a rough protype of how I wanted the site to look it was time to learn about static site generation. I chose a static site becuase I found the other option to overwhelming. The operation of a program that uses some markup to produce a complete directory of html files makes good sense to me. In addition it seems like a good idea to get familiar with jekyll since that was the system of choice for github. Couldn't be simpler to get it running, just follow the diretions here:

For a little while the serve command was my best friend. I used my protype to build a defualt layout. The about me page and a simple post index became integrated into my home page. I have yet to settle on how I want the blog and contact page to operate. As such they will remain unlinked for the moment. Adding posts is a breeze as well. 

When it came time to put my site onto bluehost things went smoothly. I used ftp to copy the _site directory over to the public_html directory, and viola, a working website!. However, I was not about to use ftp everytime I wanted changes made. 

I keep the jekyll project for my website under version control, the repository is publically available on my github. Using a git hook seemed to be the way to go as far as automating the transfer of files to the server. So i wrote a script to do it, which can be found here. Note, that I decided to use a config file to store the information about username and passwords for ssh access. While this is not the most secure option, the config file should be relativley safe on my machine. Please contact me if this solution rubs you wrong and you know of a better option. 

Just to be clear, the config file looks like this:
```bash
#!/bin/bash
sshuser=user
domain=example.com
password=12345
```

Then on a pre-push commit I run the following:


If you check out the script on github you may have noticed my extensive use of the the printf. For personal scripts I make extensive use of printf becuase it allows me to color and formate text better than echo. I like to use print statements like comments since they need to provide readibility to the code, and understandability for any errors that occur during the running of the script.  

