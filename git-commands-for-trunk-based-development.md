# Git Commands for a Trunk-Based Workflow

Navigating Git in the world of trunk-based development can be a whole new ballgame, even for seasoned software engineers.
After years of pushing to a branch, creating pull requests, and wrangling merge conflicts, companies are shifting gears and returning to the familiar practice of committing directly to main.
It’s a nod to simpler times, but with a modern twist of efficiency and collaboration.

## Embrace the Simplicity:

With trunk-based development, you say goodbye to countless feature branches and pull requests.
Instead, you commit directly to the main branch, reducing complexity and maintaining a clear, linear history.

## Perfecting Commit History

Adopt the concept of micro-commits.
Instead of bundling multiple changes into a single commit, break them down into smaller, logical units.

1. `git status`: Before you commit, check the status of your changes. This gives you a clear picture of what's going into your commit.
1. `git add -AN`: Use this to add new (empty) files to your git index without staging their content.
1. `git add -p`: Now you can pick and choose exactly which changes you want to include in your commit.
1. `git commit`: With your changes staged, commit them with a concise message. 
This ensures your commit history is clean and coherent.
1. `git pull -r`: The "r" stands for rebase, and this command is your go-to for fetching remote changes and integrating them seamlessly into your local codebase.
Rebasing helps keep your [git history linear](https://onlywei.github.io/explain-git-with-d3/#rebase) and is an incredibly helpful tool to have at your disposal.
1. `git push`: Get your work up onto Development quickly.
The sooner your commits are on main, the easier it is for other devs to keep their work conflict-free.

## Streamlined Branch Management

Sometimes, you need to work on smaller tasks or experiments that don't belong on the main branch just yet:

1. `git switch -c <branch>`: Create a new short-lived branch for your focused work.
1. `git switch main`: Switch back to the main branch when you’re ready to update your branch
1. `git pull -r`: Get the latest from main
1. `git switch -`: switch back to your branch using the handy `-` command
1. `git rebase main`: Catch your branch back up to the progress of the main branch to ensure it integrates smoothly. 
This streamlines your commit history, making it cleaner and more linear.

If you’re not familiar with [rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase), here’s a quick diagram of what’s going on.
In the upper portion, we see main has two commits before a second branch, feature, has been created.
If we keep working in a silo on feature, we diverge further and further from main, giving us a higher percentage of merge conflicts with every commit.
If we run git pull -r, we grab newer commits off of main without changing their SHA.
The SHAs on feature do change, as they’re replayed on top of the commits we just grabbed off main.

[gitWorkflow](gitWorkflow.mermaid)

Sometimes, when we have a lot of commits in our history, rebasing main can cause a lot of painful merge conflicts. 
One good way to avoid a laundry list of errors is to squash commits together if they don’t need to be isolated. 
See the “Get Rid Of Those WIPs” section below.

## Forgot Something?

You just made a commit, but now your linter is calling you out on a mistake.
Want to pretend like it never happened?
If you haven't yet pushed your work, the easiest thing to do is update your code, then do a quick alteration of your previous commit.

1. `git commit --amend --no-edit`: This keeps your previous commit, but adds in new changes.

As mentioned above, only do this if you have not yet pushed your work!
Doing so creates a new SHA which will lead to teammates having conflicts on their branches.
Things can get messy and hard to unravel quickly.

## Stashing Your Work To Switch Context

You're in the middle of a feature, but not to a point where you're comfortable committing.
You have a meeting coming up, and you need to demo work from your machine.
`git stash` to the rescue!

1. `git stash`: This command allows you to temporarily set aside changes.
It saves all of your changes and resets your IDE to the last commit.
Use the -u flag if you have untracked files you’d like to stash.
1. `git stash pop`: When you're ready to resume focus on your previous work, pop it out. 

### Bonus Stash Tip

If you utilize `git stash` often, sometimes it can get confusing: "Did I already stash? Is it going to bring back the changes I need?"
You can view your list of stashed items with `git stash list`, but it may just look like a weird, unreadable list.
For clarity, you can use `git stash push -m "<message>"` to add the equivalent of a commit message to your stash.
Example: `git stash push -m "Started building message publisher"`.
When you're ready to pop it out, use `git stash list`, find your work (i.e. stash@{1}), and pop it with `git stash pop <number>`.

Another option for stash management is to actively keep it clean - when you know there’s no work in your stash that you still want, clear it out with `git stash clear`.

## Get Rid Of Those WIPs

Sometimes it makes sense to do WIP commits on a short-lived branch, but we do _not_ want those on our commit history.
For this we can use an interactive rebase: it lets you reorder, combine, or tweak commits before they become part of the main branch.
To clean up your history, figure out how many commits you need to alter:

```bash 
git log
git rebase -i HEAD~n (where `n` is the number of commits to change)
```

Go in and squash, pick or reword. For example if I wanted to change my last three commits, I would run `git rebase -i HEAD~3` and get this prompt:

```
pick 2e95a2f WIP
pick 43adabf Add user ID
pick 3facb2a WIP

# Rebase 2e95a2f..3facb2a onto 2218aa7 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
```

The bottom result is your most recent commit. 
To clean up my history, I would go edit the file to squash all commits into the first one:

```
pick 2e95a2f
s 43adabf
s 3facb2a Add cookie for visitors
```

Once it's saved (`:x`), running `git log` will show you a nice clean history that's ready to be pushed to the rest of your team.
You’ll have the opportunity to give it one clean commit message at the end.
This is the ultimate tool for refining your commit history.

Pro-tip: To avoid replacing the word ‘pick’ over and over again in VIM when doing a large rebase, try this command: `:2,$-29s/pick/s/`

## One-Off Commands

1. `git log`: Your window into the history of your project. With git log, you can see commit messages, authors, dates, and a lot more.
It's like a storybook of your project's journey.
1. `git log --graph --oneline --all`: This command adds a visual twist to git log, presenting your project's history in a more understandable and engaging manner.
It's like a family tree for your code.
1. `git reset --soft HEAD~`: A nifty trick to uncommit changes from the most recent commit without tossing them out the window entirely.
This command gives you a chance to rework things before making that next commit.
Also great if you want to see what was changed in the previous commit: run this and check the diff.

There are three possible flags to pass to git reset with different severities.
* –-soft will keep the changes staged. 
* –-mixed is the default - it will keep the changes in the working directory, but not stage them. 
* –-hard will trash your changes altogether. 
Be careful with –-hard as it will also obliterate local commits that haven’t been pushed yet.

1. `git clean -df`: A command that cleans the working directory, removing untracked files and directories. 
Great for getting rid of code you want to discard before pulling in changes.

Trunk-based development is more than just a change in workflow: it's a paradigm shift in how teams collaborate,
communicate, and build software.
By embracing a more direct approach to committing changes, utilizing these essential Git commands, and focusing on
constant integration, teams can streamline their development process, leading to cleaner code, smoother collaboration,
and faster delivery.