# University of Central Missouri
# Upsilon Pi Epsilon Git Workshop Script

# 0. Prep

You should have Git on your machine installed via the instructions we sent out.
You also need to have an account on Github.com.

On Windows, open Git Bash application.
On Mac, open Terminal application.

# 1. Help
```
$ git help                        # General help
$ git help --guide                # Common Git guides
```

# 2. Configuration
Get help on configuration
```
$ git help config                 # See FILES section
```
Show the current configuration for all projects
```
$ git config --global --list      # ~/.gitconfig
```

Check specific config values

```
$ git config user.name
$ git config user.email
```
Set your user name
```
$ git config --global "Joe Cool"
```
Set your user email
```
$ git config --global user.email joe.cool@example.com
```
Set default branch to main
```
$ git config --global init.defaultBranch main
```
Open the config in editor, note default editor is vi!
```
$ git config --global --edit      # Open config in editor
```
Set editor to Visual Studio Code: (assumes command line for VSCode enabled)
```
$ git config --global core.editor "code --wait"
```
# 3. Git 101

Change to location of your choice  
...so we can make directory for today  
Note, Mac Terminal will open to your home directory which is located in the directory /Users/username  
Note, Windows Git Bash will open to your home directory C:\Users\username
```
$ mkdir git-hands-on
$ cd git-hands-on
```
Let's create a new repository
```
$ cd git101
$ git init
```
Can also create directory and init with one command "$ git init git101"  

Let's see what we have - We list a directories contents in Unix with 'ls' command  
No non-hidden files
```
$ ls
```
However, the local git repository data is in the hidden directory .git
```
$ ls .git
```
Of particular note: 
``` 
- .git/objects/               (empty)  
- .git/refs/heads/            (empty)  
- .git/config                 `git config -e`  
- .git/HEAD                   ref: refs/heads/master  
```
What does Git think it has?
```
$ git status
```
Let's commit something!
```
$ touch README.md                 # Create README.md
$ git status                      # Untracked files!
```
Stage the file we have created
```
$ git add README.md               # Track the README.md file
$ git status                      # Changes!
```
Commit the file
```
$ git commit -m "Add empty README"
```
Let's look at our Git history
```
$ git log
```
# 4. Fork a Github Repo

We are going to use a UCM Organization Github repo for this workshop.  
However, since all of you will not have write permissions on this repo,  
Ee are going to have you Fork the repo into your own Github account.  
  
Make sure you are logged in to your Github account.  
  
In your web browser go to:  
https://github.com/ucmo-cs/git-hands-on.git
  
In the upper right hand area, click on the Fork Button  
On the page that loads, check that your user account is correct then,  
Click on the Create Fork button.  

You should now have a forked repo, and you can go to your version of the repo:  
https://github.com/{your_github_username}/git-hands-on.git

# 4. Clone a Remote Repository

We will get a local copy of the repo we forked to your account.  
```
$ cd ..   # Should be in git-hands-on directory
$ git clone https://github.com/{your_github_username}/git-hands-on.git
```
Let's see what we have
```
$ cd git-hands-on
$ ls
```
What does Git think it has?
```
$ git status
```
What local branches do we have?
```
$ git branch
```
What is 'origin/main'?
```
$ git remote -v
$ git branch -r
```
Git history
```
$ git log
$ git log --oneline --graph
```

# 5. Push to a Remote Repository

Change the test.txt file to be:
```
This is the first line.
My second line.
```

Then lets stage our changes in Git:
```
$ git add test.txt
$ git status
```
What if we change our minds before we commit?
```
$ git restore README.md
```
Change the test.txt file to be again:
```
This is the first line.
My second line.
```
Stage and commit the changes this time
```
$ git commit -m "Add second line to test.txt"
```
Look at what Git thinks it has now
```
$ git status                      # Branch is ahead
```
Look at the history
```
$ git log
$ git log --oneline --graph
```
Let's share our favorite number
```
$ git push
```
Check on web interface that changes are there.  
  
We can go back to any point in our history with checkout
```
$ git log
$ git checkout {hash number of commit one before latest}
```
Look at the file now, it should have contents before our last change:
```
$ cat test.txt
```
Note we can achieve the above with:
```
$ git checkout HEAD~1
```
Go back to the latest on the current branch
```
$ git checkout HEAD
```
# 6. Working Locally with Branches

Start work on feature 1
```
$ git branch feature1             # Create branch from here
$ git switch feature1           # Switch to different branch
$ git branch
```
Check we are on correct branch, then create and empty file, stage and commit it.
```
$ git status
$ touch feature1.md
$ git add feture1.md
$ git commit -m "Add feature 1"
```
Check file
```
$ ls
```
Switch back to main and look at files in repo
```
$ git switch main
$ ls
```
Start work on feature 2
```
$ git branch feature2             # Create branch from here
$ git switch feature2           # Switch to different branch
$ git branch
```
Edit test.txt for the feature to be like:
```
This is the first line in feature2.
My second line.
The third line.
```
Use git diff to look at the changes that haven't been committed
```
$ git diff test.txt
```
Commit the changes for feature2
```
$ git add test.txt
$ git status
$ git commit -m "Edit test.txt for feature2"
```
Check on the diffs between two branches
```
$ git diff feature1..feature2
```
# 7. Working Remotely with Branches
First get latest from GitHub, other people may have made changes
```
git fetch --all
```
Now let's share some code, our feature1.
```
$ git push origin feature1
```
We can also push the current branch (feature2)
```
git push origin HEAD
```
But notice we're missing "up-to-date"
```
git status
```
And plain `git push` fails...
```
$ git push
```
Add -u to set the local feature2 branch's upstream
```
git push -u origin HEAD
git status
```
Now plain `git push` works
```
$ git push
```
Also, typing is overrated (pc = push current)
```
git config --global alias.pc "push -u origin HEAD"
git pc
```
Now let's pretend someone else is working  
On your fork on GitHub:  
1. Use **Branch** dropdown to switch to feature1
2. Click on `test.txt`, then the pencil to edit
3. Change something and commit changes directly to `feature1`

Nothing changes until we explicitly fetch:
```
$ git branch -r
$ git fetch --all
```
We should also bring in changes from feature1
```
$ git checkout feature1
$ git status                 # behind!
$ git pull                   # get latest & merge (no conflicts only 1 side changed)
```
# 8. Merging
Assume we are done with implementing feature2  
We want to merge our code back into the main branch so that we can release it.  
  
First though, while we were working on feature2, someone else changed the main branch.
```
$ git switch main
```
Let's edit test.txt in the main branch to simulate that:
```
This is the first line in main.
My second line.
Good things come to those who main.
```
Lets remind ourselves what the two versions look like:
```
$ cat test.txt
$ git switch feature2
$ cat test.txt
```
Now let's try to merge the branch locally
```
$ git switch main      # We always switch to the branch we want to merge INTO
$ git merge --no-ff feature2   # And we specify the branch we want to merge FROM
```
That failed and left us with the merge in process.  
Git tried to combine our versions, but both branches modified the same files,  
and more specifically the same lines, so it couldn't do it automagically, it
needs our help.  
  
Edit test.txt to resolve the merge conflict.  
When done, add the file and complete the merge
```` 
$ git add test.txt
$ git commit -m "Merged feature2 into main"
$ git log
```
