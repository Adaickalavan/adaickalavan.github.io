---
title: "Git"
---

## Tutorial
+ Tutorial on [How to Use Git and Github](https://www.udacity.com/course/how-to-use-git-and-github--ud775) from Udacity. The tutorial is free.
+ Learn about Git at [GitHub Guides](https://guides.github.com/)
    + [GitHub Flow](https://guides.github.com/introduction/flow/).
    + [Git Handbook](https://guides.github.com/introduction/git-handbook/).
    + [Mastering Issues](https://guides.github.com/features/issues/).

## Commands
1. To see git hitsory, run:
    ```bash
    $ git log --graph --oneline --decorate
    ```

1. Set up user name/email address
    ```
    $ git config --global user.name <username>
    $ git config --global user.email <mailaddress>
    ```

1. To save Git password
    ```bash
    $ git config --global credential.helper store
    ```

## Configuration
+ Setup proxy configurations for Git when using Git behind a corporate proxy, e.g., `http://10.0.0.0:8080/`. 
+ Assume you would like access all repositories via a corporate proxy, except certain repositories located at http://abc.com which needs to be accessed directly without going through a proxy. 
+ To achieve this, create a `.gitconfig` file at `~/` such that `~/.gitconfig` file contains the following.
    ```
    [user]
        email = name@email.com
        name = name
    [http]
        proxy = http://10.0.0.0:8080/
    [https]
        proxy = https://10.0.0.0:8080/
    [http "http://abc.com/"]
	    proxy = 	    
    ```