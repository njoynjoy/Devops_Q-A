``` Git Clone vs Git For ```

•	git fork creates a copy of a repository on GitHub (or GitLab, etc.) account, allowing us for changes without write access to the original repo.
•	git clone creates a local copy of any Git repository (your own or someone else’s) on your machine for development.
Here’s how you’d typically use both:
1.	Fork the repo on GitHub (creates a copy under your GitHub username).
2.	Clone your fork locally using:
git clone https://github.com/your-username/the-repo.git
So: Fork = GitHub-level action, Clone = Local machine-level action.

``` Difference between Git fetch and git pull? ```

Git fetch is like, "What are the changes that were changed by the other users?" We can fetch and add to local.git from there, we can add to our code if it is not affecting our code, then we can add it through git merge or git rebase.
Git pull does all the above things like fetching, + git merging or re-basing.
EXAMPLE:
	git checkout releasefeature2
	git fetch
	git log origin/releasefeature2
	git merge
o/p---
-- Shell
Schell Scripts
##devops learing is easy ??##

	git pull origin releasefeature2
o/p—
-- Shell
Schell Scripts
##devops learing is easy ??##
###ONLY mindset and practise makes it easy###

``` What is the difference between Git Merge and Git Rebase? ```

To integrate two branches, we'll be using git merge or git rebase.
Git Rebase: when integrating from the main branch, all the commits from the main branch will be shown first, then the second branch or the feature branch which is integrating to the rebase is attached as the next. This process of integration is called linear commit Approch.
“Git merge combines two branches and creates a new merge commit while preserving the full history.”
Git Merge: in Git merge, all the commits are added as per the date or history of the commits that they are made.
“Git rebase moves your branch commits to the top of another branch. Creating a cleaner and linear history.”

```	Explain the Git branching strategy used in your company? ```

We have main branch, feature branch, release branch and hotfix branch
Main branch—where all the developers work on this branch and make the commits
Feature branch—if developers are working in any particular feature then feature branch is created.
Release branch— There will be a code freeze at this phase after all the development is completed will create tag and release into production
Hotfix branch—when there is issue in production then the hot fix branch is created and after fixing will create a tag and move to the production after the issue is fixed then hotfix branch is pushed to the release branch and also main branch.

```	3 challenges that you faced with Git in your work experience ```

1.Git branching strategy-
 
2.Access control— In an ideal scenario, we need to decide: 
•	Who has read access
•	Who has write access--- in write access also Who can do the code push, who can create branches?
•	Reviewer access
•	Maintainer access
•	Admin access

3.	.git,.git ignore and webhooks—When I joined the organization, developers were pushing unwanted files. So the review process was difficult. So, introduced pre-commit hooks, post-commit hooks and make sure the developers do not push sensitive data. And .git ignore pushing changes to unwanted files and explained developers how to use webhooks if they need webhook will request the devops engineer team we will create

```	Talk about a challenge that you faced decent times related to Git? ```

Access control is the biggest challenge because when I joined the organization, like every developer can create the branches, so there are many branches created when they do the commit. It will be running the CI/CD pipelines and it is causing the cost to the organization. There are many stale branches as well which are not used for longer than 6 months to 1 year. I have created the access like: 
•	Read access
•	Who can push that code
•	Who can create the branches
•	Reviewer access
•	Maintainer access
•	Admin access
for all the repositories.

``` How do you handle merge conflicts? ```

When one of the developers made a change in the main branch, and if another developer working on the branch made the other change. When merging the change, you will get an error like there is a merge conflict.
Avoid this much conflict. Like, we need to come and get with the developers that there is a much conflict in the code. Can you please check and resolve it. If it is with me that much conflict, I will sit with the other DevOps engineer and resolve.
 

```	Explain our and their merge strategies? ```

When we get merge Conflicts there are our and theirs
Accept the current change is ours and accept the incoming change is theres sometimes we will accept both in rare cases

Idea case is communicate with developers and resolve it.
 


```	Have you ever used Git tags? If yes, explain how? ```

Git tags are specific points in your git history typically to label the releases or bookmarks of a specific tag in your Git history.
 

```	How do you combine multiple commits into a single commit ```

git rebase -i HEAD~3 – last 3 commits were combined in to 1 commit we need pick one and sqush the rest two it will also ask for a commit message.

``` 10 Git commands that you use daily in your work? ```

1.Git Clone: to clone the repository from remote to local.
2.Git status: to get the current status, like which branch I am working on, or which remote branch my branch tracking
3.git add: to stage the files or made some changes in the code will stage those changes with git add <name of the file> or git –add all to stage all the files
4. Git commit: is used to commit or  the save the changes that we have worked on.
5.git push: if ready with the changes, then we can add and commit then, and finally push the code to the remote repository.
6. git pull: once I used git clone I will have a copy of the repo but still there are some changes then we can use git pull to pull the latest changes.
7. git checkout-- if we want to change from one branch to another for working.
8. git branch: to use all the branches or the current working branch.
9. git merge: to integrate or merge the changes from one branch to another branch.
10. git log: to view the commit history on my local machine

```	How to ignore the changes in files or folders in Git?? ```

.gitignore is file that tells git which files or folders to ignore so we don’t accidentally push the secrets, logs, build artifacts.
Vim. gitignore
 

``` What is the purpose of a.git folder in git repository? ```

.git folder stores all the information about the git repository like commit history, Branches, configuration, tags, remote 
The project  is no longer git repository if no longer .git exists where we will loose all the version history and track in a nutshell .git is a brain of your git.

``` Can you restore the deleted .git folder? ```

Yes only you have a backup or clone of the repository, but all the version history in your local will be lost.

```	A secret is pushed to git accidentally, how can you handle it ```

1—identity the path 
2—commit to remove the secret – revoke the secret 
3—BFG repo cleaner cleans the secret commits also from the commit history.
4—ask developer to rotate the secret.
5—will develop a pre-commit hook so that the if someone tries to push the secret it will not allow
6—educate developer not to push
7—create a .gitignore file and add secret.yaml so that it will not push to the git repository.
