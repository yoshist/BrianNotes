<style>
body {
    max-width: 100% !important;
}
</style>

Git Usage
=========

> **Note:**
>
> You may hear about GitHub Desktop, or see the Git integration in VS Code.
> However, the team has found that, at least for GitHub Desktop, the code can
> break. To avoid such occurences, please do not use GitHub Desktop. Reading
> through this can also help give you more control over Git.

To use Git, you must first install it. You can go to the [Git website](
https://git-scm.com/) to install it. Follow the instructions to
install Git.

# Consider remaking this part

> ## Creating a GitHub Account
> 
> The team uses GitHub as its primary Git storage. Make a GitHub account by
> going to [GitHub](https://github.com/). Follow the instructions until you have
> made an account.
> 
> 
> 
> Open up a Command Window with the following steps: Click on the Windows Start
> Icon and in the search bar, start typing “Command Prompt”. The Command Prompt
> icon will appear under “Best match”, so click it to open up a Command Prompt
> window (or open a Terminal window on Mac).
> 
> From the Command Prompt window, enter the following:
> 
> ssh-keygen -t ed25519 -C <github email>
> 
> where <github email> is the email you used to create your GitHub account
> 
> For example:  ssh-keygen -t ed25519 john.doe@gmail.com
> 
> You’ll be prompted to enter the following:
> 
> Enter a file in which to save the key:  hit enter to accept the default saved
> location.  Default location can be found here:
> 
> On Windows, it’ll be saved to C:\\\\Users\\\\YOU\\\\.ssh\id_ed25519
> 
> On MacOS, it’ll be saved to $HOME/.ssh/id_ed25519
> 
> Enter a passphrase (empty for no passphrase):  if you enter a passphrase,
> you’ll need to remember and enter it each time you push up your changes. Hit
> enter twice if you do not wish to enter a passphrase.
> 
> Your SSH key pair files are created.
> 
> Note: By default, File Explorer in Windows does not show file extensions. To
> make the file extensions visible, select View -> Show -> File Name Extensions.
> If the folder C:\Users\YOU\.ssh is opened in File Explorer, there will be two
> files with the name id_ed25519, one with the extension .pub and one without
> any extension, as shown in below:
> 
> The two files are the public and private key pair used for user
> authentication.
> 
> The file with the .pub extension is the public key that will be used when
> uploading files, or setting up GitHub.
> 
> The file without the .pub extension is a private key, and should NOT be shared
> with anyone.

## Git Overview

Git is a version control system. This means that, for a workspace of code (a
*repository*), it stores snapshots of what the code is at certain points. You
may be used to this concept from applications like Google Docs, in which you can
look at the history of the documents and restore past changes. Git is similar,
but it has more features and gives you more control over each snapshot.

Git manages directories called *repositories*. Any directory can be made into a
Git repository. To make a directory into a repository, do `git init`. This
creates a subdirectory named `.git`, which contains all of the information that
Git uses.

Git does not store every version of your repository, as this would take too much
memory. Instead, it stores a list of differences, or *diffs*, between versions.
By adding up all of the diffs, the current file can be remade from the original
one.

**Refer to this image for the following explanations:**
> ```
>      ┌C─E┬F─ 
>  A─B─┤  ┌┘   
>      └─D┴──G─
> ```

The Git system is usually likened to a tree. Each of the nodes on the graph, the
letters, represent a *commit*, or a snapshot of the repository. Commits are
meant to be static, not changing and not dependent on future commits.

The paths on the graph should be read from left to right, starting from node A.
The repository is initially at commit A. Then, a commit B is made. At this
point, the repository contains two snapshots of the history, along with the
current file state. The current file state is called the *working tree*, as
nothing has been committed yet.

After commit B, the path splits. This is called a *branch*. In a branch, the
changes made are isolated from those in another branch. After commits C, D, and
E are made, the only changes in the top branch are those made by C and E. In the
bottom branch, only the changes from D are present. This allows multiple people
to work on the same project without interfering with each other.

Although branches are convenient, they are not useful unless you can somehow
recombine the changes made. As it turns out, you can do so with a process called
*merging*. You can merge the changes of one branch into another branch, which
will add the diffs together. However, sometimes the diffs may conflict, in which
case you have to resolve the merge manually, choosing what changes to make. We
will talk more about merging later.

The line after D represents merging the bottom branch into the top branch. At
commit F, all of the changes from A-E are present. However, at commit G, only
the changes from A, B, and D are present.


## Basics

Running the command `git status` will give you the status of the current git
repository. Try to run this on the command line. It should give you a message
like this:

> ```
> fatal: not a git repository (or any of the parent directories): .git
> ```

Why did this happen? It is because we are not in a Git repository. We are only
in a Git repository if the current directory, or any of its parents has, as a
direct child, a `.git` directory.

As stated before, you can make a directory a Git repository by running `git
init`. Create a directory in a good location, perhaps putting it in a directory
for robotics. `cd` to it in the terminal, then run `git init`. 

Now you can run `git status` again. You should get a message like this:

> ```
> On branch main
> Your branch is up to date with origin/main
>
> nothing to commit, working tree clean
> ```

There is no failiure this time, so we are now in a Git repository. But what does
each line mean?

The first line states that you are on the branch `main`. Although this is the
default branch, you should not work on this branch. The `main` branch is meant
for releases, not normal working. To enforce this, pushing to `main` on GitHub
is not allowed, and pull requests require a review. We will cover more about
this later.

The second line states that the current workspace has all the changes that
`origin/main` does. `origin` can be thought of as the files on GitHub. However,
it does not automatically update along with GitHub. This means that when
updating GitHub, your computer will still have the old files until you
explicitly tell it to update. Please remember this, as you may think that you
have all the changes, but you will not know unless you update your repository
again.

The last line says that you have no changes that are not committed. We will also
cover this topic later.


## Committing

### How to commit

As stated before, commits are snapshots of your code. To make a commit, run the
command `git commit`. However, if you have been following this tutorial, when
you run this command, you will get the following message:

> ```
> On branch main
> 
> Initial commit
> 
> nothing to commit (create/copy files and use "git add" to track)
> ```

This message is in the same format as that of `git status`. The last line states
that there have been no changes since the last commit (the initial commit). Try
creating a file called a. Now, when you run `commit`, you should get the message

> ```
> On branch main
> 
> Initial commit
> 
> Untracked files:
>   (use "git add <file>..." to include in what will be committed)
> 	a
> 
> nothing added to commit but untracked files present (use "git add" to track)
> ```

Note that the commit still does not work, but new information is shown. To
explain this, we will need to understand how Git tracks files.

### File tracking

Git remembers what files have been added to previous commits. Note that Git only
saves **files**, not **directories**. When you change a file's location or name,
it treats this as a special operation, rather than the creation and deletion of
whole files. This means that you cannot add an empty directory to a Git
repository. The directory structure in Git is only implicitly made by the files
you add, so if you have a directory containing only files that have not been
saved, then the directory containing them will not be saved either.

When you add a file to a commit, the file is *tracked*. When you run `git
status`, you can see three categories of files: untracked, unstaged, and staged
files.

1. Staged files are files that have been prepared for committing. We will talk
   more about staging files later.

1. Unstaged files are files that have been inside a commit before, but the
   changes have not been staged.

1. Untracked files are files that have never been added to a commit and are not
   staged.




## Branching

As stated before, you can make branches in Git. 


## Cloning

You know how to make your own repository, but what if you want to use the
team's? You can do so by *cloning* it. Git by itself cannot share files, but by
using a host site like GitHub, we can share a single repository across the whole
team.

To clone a Git repository from GitHub:

1. Go to the repository on GitHub

1. Click the arrow on the green 'Code' button, and look under 'Clone'

1. Some options may appear:

   1. HTTP

   1. SSH

   1. GitHub CLI
  
   * If you are on a Unix-based system (typically anything but Windows), use
     the link for SSH.

   * If you are on Windows, copy the link for HTTPS. The option for SSH may not
     show up or work.

1. Open your terminal, and `cd` to the directory into which you want to clone
   the repository.

1. Enter `git clone `, and then the link that you just copied. There should be a
   space between `clone` and the link.

   If your screen freezes for a long time, then press `<C-c>`. This will stop
   the execution. This will likely occur if you are connected to the school wifi
   and using SSH. We are not sure how to solve this, but you can connect to your
   phone's hotspot and run the git commands.

   If you do not want to waste data downloading the whole database, you can do
   the following:

   1. Retry cloning using the HTTPS link.

   1. Go into the repository. You can do this from the terminal, or use the
      native file explorer.

      * For Unix, do `ls -la`. This shows hidden files and displays them in a
        readable list

      * For Windows, just do `dir`.

   1. Go into the `.git` directory. This directory is very important. You should
      not try modifying anything in here without knowing what you are doing.

   1. Edit the `config` file. This stores the configuration for the current
      repository.

   1. Under `[remote "origin"]`, change `url` to the SSH link. This makes the
      access link the correct type.

   Although you can clone through HTTPS, you will likely not be able to do much
   more with it, unless you are on Windows.

Now that you have cloned the repository, there is a **copy** stored on your
machine. Note that this is just a copy. If you want to have your changes on
GitHub, you have to update it manually. We will talk more about updating later.
