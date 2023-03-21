---
layout: post
title:  "Rebasing: A Git Tool for Cleaner and More Organized Commit History"
date:   2023-04-04
categories: git rebase reflog log oneline commit history
---

If you're a developer working on a project using Git, you may have come across the term "rebasing" before. Rebasing is a powerful Git tool that can help you clean up your commit history and make it more organized.

## What is Rebasing?

Rebasing is the process of moving a branch to a new base commit. This means that instead of merging changes from one branch to another, you rewrite the branch's commit history to make it appear as if it was created from the new base commit.

Rebasing can be a useful tool when you're working on a branch that's been in development for a while and has many commits. Rather than having a messy and convoluted commit history, rebasing allows you to create a more linear and clean history. This makes it easier to understand what changes were made and when they were made.

## How to Use Rebasing in Git

Here's how you can use rebasing to clean up your commit history.

### Check Commit History

Before you start rebasing, it's a good idea to check your commit history. You can do this by running the following commands:

```bash
git reflog
git log --oneline
```

These commands will show you a list of commits in your branch, along with their commit messages. Use this information to identify which commits you want to keep and which ones you want to get rid of.

### Start Interactive Rebasing

Once you've identified the commits you want to keep, it's time to start interactive rebasing. You can do this by running the following command:

```bash
git rebase -i <after-this-commit>
```

Replace `<after-this-commit>` with the commit before your commits started.

### Mark Commits
Once you run the interactive rebasing command, you'll be prompted with a list of commits. You can mark each commit with one of the following commands:

    pick (p): use commit
    squash (s): meld into previous commit
    reword (r): use commit, but edit the commit message
    edit (e): use commit, but stop for amending
    fixup (f): like "squash" but keep only the previous commit's log message
    exec (x): run command (the rest of the line) using shell
    drop (d): remove commit

Use the "pick" command for the commit you want to keep, and the "squash" command for the commits you want to meld into the previous commit.

For example, if you want to keep the commit with the hash a25e641 and meld the commits with the hashes a25e642 and a25e643 into it, you would mark them as follows:

    p a25e641 commit msg
    s a25e642 commit msg
    s a25e643 commit msg

Once you're done marking the commits, save the file.

### Edit Commit Messages

After marking the commits, you'll be prompted to edit the new commit message for the melded commits. Edit the commit message as needed.

### Force Push

Once you're done editing the commit messages, you're ready to force push your changes. You can do this by running the following command:

```bash
git push -f
```

This will overwrite your branch's commit history with the new, melded commits.

## Final Thoughts

Rebasing can be a powerful tool for cleaning up your Git commit history and making it more organized. However, it's important to use rebasing with caution, as it can cause conflicts and make it difficult to merge changes from other branches. It's also important to communicate with your team.
