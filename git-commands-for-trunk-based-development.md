# Git Commands for a Trunk Based Workflow

Navigating Git in the world of Trunk Based Development can be a whole new ballgame, even for seasoned software
engineers.
After years of the familiar routine of pushing to a branch, creating pull requests, and wrangling merge conflicts,
companies are shifting gears and returning to the classic practice of committing directly to main.
It's a nod to simpler times, but with a modern twist of efficiency and collaboration.

If you're up for trying out this streamlined approach, here are some essential best practices.

### Embrace the Simplicity:

With Trunk Based Development, you say goodbye to countless feature branches and pull requests.
Instead, you commit directly to the main branch, reducing complexity and maintaining a clear history.

### Small, Frequent Commits:

Embrace the concept of micro-commits.
Instead of bundling multiple changes into a single commit, break them down into smaller, logical units.
This allows for easier code reviews, pinpointing issues, and tracking changes over time.

### Collaborative Environment:

Cultivate an environment where collaboration and knowledge-sharing thrive.
Encourage open discussions, shared ownership, and a sense of collective responsibility.

### Essential Git Commands for TBD:

1. `git status` → `git add -AN` → `git add -p` → `git commit`: This sequence streamlines the process of committing only
   the relevant changes while ensuring a polished final product.
1. `git reset --soft HEAD~`: A nifty trick to uncommit changes from the most recent commit without tossing them out the
   window entirely. This command gives you a chance to rework things before making that next commit.
1. `git log`: Your window into the history of your project. With git log, you can see commit messages, authors, dates,
   and a lot more. It's like a storybook of your project's journey.
1. `git log --graph --oneline --all`: This command adds a visual twist to git log, presenting your project's history in
   a more understandable and engaging manner. It's like a family tree for your code.
1. `git rebase`: An efficient way to integrate changes from one branch into another. It streamlines your commit history,
   making it cleaner and more linear.
1. `git rebase -i HEAD`: The ultimate tool for refining your commit history. It lets you reorder, combine, or tweak
   commits before they become part of the main branch.
1. `git pull -r`: The "r" stands for rebase, and this command is your go-to for fetching remote changes and integrating
   them seamlessly into your local codebase.
1. `git stash`/`git stash pop`: These commands allow you to temporarily set aside changes when you need to switch focus.
   Stash your work and then pop it back when you're ready to resume.
1. `git stash push -m <message>`: This takes all your unfinished changes, packing them neatly in a box, and adds a label
   to remind you what's inside. When you're ready to come back to this work, you'll know exactly what you were working
   on and why.
1. `git switch -c <branch>`: Create a new branch for short-lived stories or experiments. Later, you can use git rebase
   main to integrate your changes smoothly.
1. `git commit --amend --no-edit`: Perfect for those minor formatting fixes after committing. It helps maintain a clean
   history without clutter.

Trunk Based Development is more than just a change in workflow: it's a paradigm shift in how teams collaborate,
communicate, and build software.
By embracing a more direct approach to committing changes, utilizing these essential Git commands, and focusing on
constant integration, teams can streamline their development process, leading to cleaner code, smoother collaboration,
and faster delivery.