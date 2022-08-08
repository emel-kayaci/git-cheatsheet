# UNIX COMMANDS

`ls`: Shows files in directory. Use tab to autocomplete.
`ls -l`: Shows files in directory with additional info.
`ls -a`: Shows also hidden files.

`clear`: Clears the terminal. (ctrl-l)

`open .`: Opens directory as folder.
`open Desktop`: Opens desktop as folder. (Just an example you can pick whatever destination is needed.) 

`pwd`: Print Working Directory. Shows current location.

`cd`: Change directory. 
`cd..`: Go back one level.

`touch`: Create a file or files. (touch text1.txt text2.txt text3.py text4.pdf) To create file in different directory use touch command with path such as touch Desktop/Folder1/text1.txt

`mkdir`: Creates a new directory/folder (or directories)
 
`rm`: Delete a file or files. (Not to trash bin, permanently removes them)
 
`rm -rf`: Deletes a directory. (r: recursive, f: force)
 
# GIT COMMANDS

[Official Documentation](https://git-scm.com/docs)
 
`git status`: Gives information on the current status of a git repository and its contents.
 
`git init`: Create a new git repository. Do once per project and initialize in the top-level folder containing the project.
rm -rf .git deletes the folder that makes a folder git repository and we can check this with git status command.

## GIT ADD & GIT COMMIT

![gitLifecycle](https://user-images.githubusercontent.com/43893190/183290757-f387f46e-6f6f-4bd3-b1a0-725bc361e7d7.png)

Working Directory --git add--> Staging Area --git commit--> Repository

The commits should be atomic. A commit should encompass a single feature, change or fix. It makes it much easier to undo or rollback changes. 

`git commit`: Every checkpoint made in the project. Command to actually commit changes from the staging area.
`git commit -m "my message"`: -m flag allows us to pass in an inline commit message. If this flag is not written a text editor would launch automatically and will wait for commit message.
`git commit -a`: Add all changes of tracked files and commit them.

`git add`: Stage changes to be committed. Group specific changes together, in preparation of committing. git add file1 file2
`git add .`: Stage all changes at once.
`git rm -r --cached .`: Remove all files from staging area. `git rm --cached file1`: Removes only specified file.

`git log`: Log of commits. (press q to exit log window in terminal later)
`git log --oneline`: Shorter commit messages.

### Amending Commits

Sometimes wrong commit is made (forgotten files or typo in commit message). Rather than creating new separate commit, redo the previous commit using the `--amend` option. It is only useful to redo the mistake you did one commit ago.

If you forgot files, add forgotten files in staging area before calling command `git command --amend`. If you only want to change the commit message just call the command. 

### Ignoring Files with .gitignore

Ignoring the files and directories in a given repository using a .gitignore file. By making this change git will no longer track the changes made in those files. This is useful for files like:
- Secrets, API keys, credentials
- OS files
- Log files
- Dependencies & packages

Create a file called .gitignore in the root of a repo. (you can put it anywhere but root is common practice)
- .abc will ignore files named .abc
- folderName/ will ignore an entire directory
- *.log will ignore files with .log extension

## GIT BRANCH

Alternative timelines for a project. If merge does not happen changes made in one branch do not affect the other branches.

### Head -> master

Head is the pointer that refers to the current location in the repository. When using command git log this can be seen at the most recent commit. In the example above head tells us that we are at the master branch.

`git branch`: List of existing branches. * indicates the branch you are currently on. `git branch -v`: Last commit for every branch. 

`git branch <branch-name>`: Creates new branch upon the current head but does not switch to this branch (head stays the same). 

`git switch <branch-name>`: Switch to given branch.
`git switch -c <branch-name>`: Creates a new branch and switch to it. (`git checkout -b <branch-name>` does the same)  

`git checkout <branch-name>`: Historically, this command was used to switch branches. It is still works but unlike switch it has other functionalities.

`git branch -m <branch-name>`: You have to be on the branch you want to rename.

**Important fact:** If changes are not commited or stashed all progress would be lost during the branch switching. Files that are not in old branch but created during new branch act different. If these files are not commited then they will follow through new branch.  

### Deleting branches

[Delete branches](https://www.cloudbees.com/blog/git-delete-branch-how-to-for-both-local-and-remote)

`git branch -d <branch-name>`: Deletes the branch with given name. (commits are stil there only reference is deleted)

- Will give an error if you try the delete the branch you are currently on.  (Cannot delete branch <branch-name> checked out at 'path')

- If you switch to a different branch and try to delete the branch you tried before then different error would show up. (The branch <branch-name> is not fully merged)

`git branch -D <branch-name>`: Allow deleting the branch irrespective of its merged status.

## GIT MERGE

[Git Merge Tutorial](https://www.atlassian.com/git/tutorials/using-branches/git-merge)

- Branches are merged, not specific commits.
- Always merge to the current head branch.
- Merging does not mean that the previous branch whose information was added is lost. We can still continue to develop it as it is a separate branch.

Two steps in merge:

1. Switch (or checkout) to the branch you want to merge the changes into. 

2. Use the `git merge` command to merge changes from a specific branch into the current branch. 

### Fast Forward Merge

<img src="https://user-images.githubusercontent.com/43893190/183354974-63b2ac8f-f713-4c19-a32a-c1fd6de52585.jpg" width="380" height="500" />

### Typical Merge

Rather than performing simple fast forward, git makes a new commit on the master branch and will expect a commit message from you. This commit unlike others has two parents. 

### Merge Conflicts

Depending on the specific changes, Git may not be able to automatically merge. This merge conflicts should be manually resolved. Git changes the content of the files to indicate the conflicts that it wants you to resolve. 

- The content came from the Head branch is displayed **between the <<<<<HEAD and ====== symbols.**

- The content from the branch you are trying to merge from is displayed **between the ====== and <<<<< symbols.**

1. Open the file or files with merge conflict.
2. Edit the file or files to remove the conflict. Decide which branch's content you want to keep in each conflict. (or keep the both content) It is not just and, or operation you can type whatever you want in the file.
3. Remove the conflict markers in the document.
4. Add changes and make commit.

## GIT DIFF

- Lists all the changes in working directory that are not staged for the next commit.

- View changes between commits, branches, files, working directories and more. 

- It does not impact the repository just gives info like git log or git status command.

Explanation of the output of git diff:

`diff --git a/diff_test.txt b/diff_test.txt`: Comparison unit (which files are being compared, usually two versions of the same file)

`index 6b0c6cf..b37e70a 100644`: File metadata

`--- a/diff_test.txt`: Legend that assigns symbols to each diff input source. Changes from a/diff_test.txt are marked with a ---.

`+++ b/diff_test.txt`: Changes from b/diff_test.txt are marked with the +++ symbol.

The remaining diff output is a list of diff *chunks*. Not entire content of the file just the modified parts. 

`@@ -1 +1 @@`: Each chunk is prepended by a header inclosed within @@ symbols. The content of the header is a summary of changes made to the file.

For example if this would be like this `@@ -34,6 +34,8 @@` then it would mean that 6 lines have been extracted starting from line number 34. Additionally, 8 lines have been added starting at line number 34.

`-this is a git diff test example`: Line that begins with - come from file A.

`+this is a diff example`: Line that begins with + come from file B.

`git diff HEAD`: Lists all changes in the working tree since last commit. (git diff only includes unstaged changes, this also includes staged changes) 

It is not possible to view a newly created file either inside or outside the staging area with the `git diff` command. But if the newly created file is transferred into the staging area, we can see the change that a new file has been created with the `git diff HEAD` or `git diff --staged` command.

`git diff --staged` or `git diff --cached`: List the changes between the staging area and last commit. (shows what will be included in the commit if we run it right now)

`git diff HEAD filename`  or  `git diff --staged filename`: View the changes within a specific file. To view multiple files separate them by spaces.

`git diff branch1 branch2`: List the changes between the tips of branch1 and branch2. 

`git diff commit1 commit2`: Compare two commits by giving commit hashes. (To view commit hashes use command git log --oneline)












