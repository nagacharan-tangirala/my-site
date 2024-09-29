---
layout: post
title: How to Use Git in Overleaf
date: 2024-09-28 09:00:00
description: Git integration with Overleaf
tags: overleaf git
categories: howto
thumbnail: assets/img/9.jpg
---

## Introduction

[Overleaf](https://www.overleaf.com) is an online latex environment.
It allows for easier collaboration and online management of the documents.
As a PhD student, this is one of the integral software because I handle all of my writing with the help of Overleaf.
Document history is integral for me because of the I have collaboration with my mentor.
However, Overlaef has moved the ability to longer document histories behind the subscription tier.
Hence, an alternative way of handling this is discussed in this post.
An added benefit is the ability to work on the local machine without needing access to internet.

## Git Integration

By storing the document history in git, there is no limit to the document history.
This is easy to setup.
First, you need to generate git token.

### Git token

Click on the Account Settings and you will notice a project synchronization section.
One of the options will be Git Integration.
Click on the `Generate token` button as shown below:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/overleaf-git-28-09-2024/git_token.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is the account settings menu.
</div>

Save the generated key in a secure location, as you won't be seeing this key in the future.
If you already have a key generated, but do not know where is it, then you can delete it and recreate a new key.

### Git Synchronization

Go to the *Menu* on the top-left:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/overleaf-git-28-09-2024/overleaf_menu.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is the menu that appears on the left-side of Overleaf.
</div>

Click on the *Git* button to open the following window:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/overleaf-git-28-09-2024/git_link.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is the git link and contains a randomly generated string as the repo name.
</div>

### Local Copy

Copy the command as is to clone the project to your local machine. 
It is suggested to give a custom name at the end of the command to prevent directories with awkward names.
For example:

```
git clone https://git@git.overleaf.com/66f2717alsdjfaliwoeru <custom_name>
```

It will prompt for the username and password.
Give the same email as your overleaf account.
For password, enter the token created in the previous step.
Upon successful authentication, the command will clone the repository to the folder with name `custom_name`.

### Bonus tip

The git integration combines all the changes into one big change.
Granular history is not maintained.
This can be avoided by setting up a cron job that regularly updates the overleaf data.
This will avoid the document running into outdation.

