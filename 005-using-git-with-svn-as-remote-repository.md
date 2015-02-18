# Using GIT with SVN as Remote Repository

Migrating from SVN to Git can be a tedious and nerve-racking process, especially in environments where there is resistance or inability to change.

However, as a developer you should not suffer from this. In this post I will show how you can use Git locally and still be able to push your changes to the corporate’s central SVN repository.

For setting up and managing a local Git repository you have two options:

*   Using command line tools (difficult)
*   Using SmartGit (easy)

After you’ve setup the Git repository, you can simply use it like any other Git repository. Commits will be done on the Git repository and you can choose to push them to SVN at a later time. When you push to SVN, each Git commit will result in an SVN commit with the same commit message.

Since changes will be pushed to SVN there are some caveats regarding .gitignore files that you need to take into account. The last chapter of this post explains it all.

For this post I assume that you are a bit familiar with Git, have used it before and know how to do basic things like committing.

## Create a Git – SVN repository: The command line approach

On Linux you can install git-svn through your favorite package manager.

`sudo apt get install git-svn `

Create an empty directory that will host your Git repo.

`mkdir ~/code/myProject && cd ~/code/myProject`

Now initialize the Git repo with the SVN repository and fetch the contents from it.
Because of Git’s distributed and decentral nature it will fetch the complete SVN history.
Depending on the size of your repo this will take some time.

`git svn init URL_OF_SVN_REPO`

`git svn fetch`

When finished, your Git repository is ready. You can just use it like you would with any other Git repo.

To grab new revisions from SVN you do

`git svn rebase`

To push local Git commits to SVN you do

`git svn dcommit`

## Create a Git – SVN repository: SmartGit

[SmartGit](http://www.syntevo.com/smartgithg/ "SmartGit") is a visual tool for managing Git repositories and is available for Linux, Mac and Windows. SmartGit supports Git repositories with SVN as a remote repository. SmartGit is very useful when merging remote changes to your local repository since it offers visual diff tooling.

To clone the SVN repo open the Clone dialog (Project -&gt; Clone).

![image](https://31.media.tumblr.com/675983b677d38acfc556580055e74f3f/tumblr_inline_njvfasBUQF1t9ks7b.png)

On the next screen select the directory in which the Git repo will be created.

![image](https://31.media.tumblr.com/34aa95c5ade29ff50d0bf0445e404c0c/tumblr_inline_njvfb4aX6Q1t9ks7b.png)

After completing these steps SmartGit will download the SVN repository. After a short time your repository is already usable. SmartGit continues to download all revisions on the background and will notify you once it’s done.

Just use the repository as you normally would (in your IDE for instance). When you’re ready to push your commits to SVN, or want to fetch the latest revisions from SVN use the Push and Pull buttons from SmartGit.

![image](https://31.media.tumblr.com/f3ff9e98c19e1a773f1760027f253935/tumblr_inline_njvfbmyuuZ1t9ks7b.png)

**Gitignore and svn ignore**

SmartGit will automatically convert your gitignore directives into SVN ignore properties when you push the changes to the SVN &nbsp;repository. One thing to take into account: SVN doesn't support recursive ignores like Git does. A recursive gitignore directive will be translated to an svn ignore directive on every sub-directory. So don't be surprised if you see a lot of propset changes in SVN, it's because of this feature.
