# GitComm - A Guide to Git User Happiness

GitComm is an opinionated guide for working with Git.  Some might call GitComm a *Git Style Guide*; some might call it a collection of *Best Practices for Git*.  Some might even call it a *spec* or a *standard*.  As far as GitComm is concerned, it's just a **comm**on sense collection of ideas that the software development **comm**munity has discovered and embraced over time.  If your team values [Agile Development](https://en.wikipedia.org/wiki/Agile_software_development) and [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration), you'll want to take a good look at GitComm.

## Version Information

GitComm follows [semantic versioning](http://semver.org).  The current version is 0.1.0.

## Attribution

GitComm was created by *borrowing* ideas from many smart people.  If you see an idea and know the original source, please let us know so we can provide a proper reference.

## Contributing

Like all things, GitComm is evolving.  If you believe something should be changed or added, please consider contributing.  **Before** submitting a pull request, create an issue and format your branches/commits in a manner that looks like you've read and understood GitComm.  GitComm may be a little demanding, but it's maintainers are actually quite nice.  Positive discussions that make improvements (or attempt to make improvements) are always encouraged.

## Table of Contents

1. [A Lean and Mean Introduction](#a-lean-and-mean-introduction)
1. [GitComm's Recommended Workflow](#gitcomms-recommended-workflow)
1. [Commits](#commits)
    1. [Commit Messages](#commit-messages)
    1. [Commit Structure](#commit-structure)
1. [Branches](#branches)
    1. [Branch Naming](#branch-naming)
    1. [Rebasing](#rebasing)
    1. [Merging](#merging)
1. [Workflows](#workflows)
    1. [GitHub Flow Workflow](#github-flow-workflow-recommended)
    1. [Centralized Workflow](#centralized-workflow)
    1. [Git Flow Workflow](#git-flow-workflow)
    1. [Integration-Manager Workflow](#integration-manager-workflow)
    1. [Dictator and Lieutenants Workflow](#dictator-and-lieutenants-workflow)

## A Lean and Mean Introduction

GitComm is based on three principles:

1. Be brief.
1. Treat Git like you treat your code.
1. Simplify branching based workflows.

Everything about Git suggests brevity.  Git's name is short.  The amount of space we have to type messages is short.  The commands we use to interact with Git are short.  If you can adopt the mentality of using the minimal set of changes needed to accomplish a goal, you'll be well on your way to mastering Git and following part of GitComm's first principle - **Be brief**.

In addition to staying brief, it's also important to remember who we are when we're using Git.  As developers, ***we're supposed to*** place a premium on writing correct, maintainable, readable code.  We're supposed to be the masters of logic and organization.  Unfortunately, there's far too many of us who use Git with carelessness and sloppiness.  Instead, let's **treat Git like we treat our code**.  Let's keep ourselves organized and let's fuss over the details when we're using Git.

Lastly, you can't use Git without having a Workflow.  In the spirit of being brief, GitComm is focused on principles that **simplify branching based workflows** like [GitHub Flow](#github-flow-workflow-recommended) and [Git Flow](#git-flow-workflow).   Many of GitComm's principles are useful in other workflows, however, some principles just won't apply to workflows that aren't based on branches.

## GitComm's Recommended Workflow

The title of this section is actually misleading because **GitComm doesn't have a recommended workflow**.  There are many workflows and they all have positive and negative traits which make them better choices for some projects and not others; it's impossible to pick one and tell you to use it all the time.  That being said, GitComm is designed to work best with [GitHub Flow](#github-flow-workflow-recommended).

GitHub Flow is a fantastic lean workflow.  It's not too simple and it's not overly complex.  It works great with small and large projects, and it scales well with small and large teams. The following list explains the main aspects of GitHub Flow:

  1. `master` is the release branch and it should always be deployable.
  1. `master` also serves as the integration branch.
  1. Developers create feature branches off `master`, do some work, then merge the feature branches directly into `master` when the work is "done".
  1. Feature branches should never be merged into `master` if they contain known problems.
  1. While developers are working in a feature branch, they regularly rebase their feature branch to keep it in sync with `master`.  They can also rebase onto another feature branches if they need to pickup work being done by someone else (however, doing so means the other feature branch must be merged to `master` first).
  1. Prior to merging a feature branch into `master`, developers always perform a rebase.  This ensures their code is up-to-date and more likely to be code reviewed off the current state of `master`.
  1. Before a feature branch is merged into `master`, it must successfully pass through all quality control checks.  This depends on the organization, but it could mean things like 1) making sure all automated checks pass, 2) making sure a second developer has completed a code review, 3) making sure security audits are complete 4) making sure user acceptance testing passes, etc...  The sky is the limit here.  There are hundreds, if not thousands, of things to automatically check.
  1. It's useful to use **Pull Requests** when merging a feature branch into `master`.  GitHub (and other Git hosting sites) can be configured to trigger hooks which alert Jenkins (or other build tools) to perform automated checks.  Pull Requests can be blocked until all checks pass.
  1. Changes should have a small scope so their corresponding feature branch doesn't stick around to long.  This helps reduce merge conflicts, and conflicts with other feature branches that are being worked on in parallel.
  1. If problems are found after merging into `master`, another feature branch is created and the issue is addressed.
  1. There's no `develop` branch, no `hotfix` branch, and no `release` branch. At some point, `master` is tagged and a release is deployed.
  1. Developers can merge an incomplete feature branch into `master`, as long as `master` stays deployable, there are no bugs, and the user isn't negatively impacted.  UI elements can be hidden behind feature toggles to keep incomplete work hidden from users if needed.

In GitHub Flow, `master` is seen as a steady stream of deployable code. GitHub Flow is very focused on merging **high quality** feature branches, and, as a result, there's no need for integration branches in this workflow since the **individual** feature branches are vetted before they can be merged.

Now that we've described the workflow we have in mind, it's time to move on to the list of best practices. Without further adieu, let's talk about commits ...

## Commits

### Commit Messages

- **Create commit messages with the following three parts:**

    ```shell
    1.  A short sentence/phrase called "The Summary Line"         # [REQUIRED]
    2.  A single blank line called "The Blank Line"               # [REQUIRED, IF THE BODY IS PRESENT BELOW]
    3.  An optional paragraph of text called "The Body"           # [OPTIONAL]
    ```

  An example commit message looks like this:

    ```bash
    This Summary Line is less than 50 characters

    This is The Body paragraph and it follows the single blank line above.
    The Body can have as many lines as it needs and it can contain periods
    at the end of a line.  Since The Body is present, there must be a
    blank line between The Summary Line and first line in The Body.
    A line in The Body can use up to 72 characters.
    ```

- **Limit The Summary Line to 50 characters.**  This limit allows the subject to be fully readable in most (if not all) of GitHub UIs and it forces authors to be concise.  GitHub itself warns about exceeding 50 characters in some UIs, so this keeps the commits messages consistent with GitHub.

    ```shell
    # good                         # bad - exceeded 50 characters and was downright rude
    Made an effort to be brief     Could care less if you can only see part of this commit message
    ```

- **Begin The Summary Line with a capital letter.**  Are we elementary school students?  No!  We're highly trained engineers who capitalize sentences (and partial sentences) properly.

    ```shell
    # good                         # bad - started The Summary Line with a lowercase letter
    Remove dead code               remove dead code
    ```

- **Use single quotes to highlight words & phrases in The Summary Line (and The Body).**

   ```shell
   # good                          # ok
   Use rails '~> 1.3.0'            Use rails ~> 1.3.0
   ```

- **Be specific about what the commit does in The Summary Line.**

    ```shell
    # good                                # bad
    Add 'Forgot My Password' page         Enhance sign in code
    ```

- **DO NOT write lazy messages in The Summary Line.**  Lazy commit messages tell other people you don't care.  As a developer, if you don't care about the little things (like a commit message), why should anyone believe you care about the correctness of the code you just wrote either?  Developers need to sweat the details EVERYWHERE.  Fortunately, you're not one of *those guys* since you're reading this and leveling up your skills.

    ```shell
    # bad
    431431  Checkpoint     # Useless, since every commit is inherently a checkpoint
    34ardr  Fixed it       # Fixed what exactly?
    a34314  Started work   # Doesn't explain what was done
    ```

    ```shell
    # good
    431431  Cleanup javascript code styling            # Now we know what happened
    34ardr  Fix new registration creation bug          # Now we know what happened
    a34314  Create scaffolding for events controller   # Now we know what happened
    ```

    Having said that, it's ok to create temporary commit messages if you plan on squashing the commits before sharing with others.

- **DO NOT end The Summary Line with a period (or other punctuation characters).**  Raise your hand if you think the 50 character line limit gives you space to waste characters on a crummy period.  If you raised your hand, look around; you're the only one with your hand in the air.

    ```shell
    # good                        # bad - ended The Summary Line with a period
    Fix login timeout             Fix login timeout.
    ```

- **DO NOT ask questions in a commit message.**  Got it?  Good.  Git commit messages aren't the place to raise new issues; they are used to inform others (or yourself) about the changes made by the commit.  Put the questions in an issue tracker where they're more likely to receive the proper attention.

    ```shell
    # bad - asks a question in The Body where it probably won't be responded to (if it's even seen)
    Warn user about incorrect password on sign in

    I updated the HTML respond with a warning if the password
    doesn't match.  Should we switch to the devise library so
    we can get some of this capability for free?
    ```

- **Use the imperative mood on The Summary Line.**  When someone speaks in the imperative mood they're typically making commands or  requests.  The imperative mood communicates what **WILL** happen if a commit is applied (instead of treating the commit like something that happened in the past).  Commit messages written in the imperative mood are shorter, which is helpful when trying to fit thoughts into 50 characters or less, and they mimic Git, which uses the imperative mood when it auto generates commits (e.g. when it generates a merge commit).  GitHub also uses them in multiple UIs.

    ```shell
    # good - speaks with authority, just like GitComm
    - Add helper methods to the sign in module
    - Merge branch '15-fix-sign-in-bug'
    - Raise code coverage to 95.6%
    ```

    Use words like `Add`, `Fix`, `Make`, `Refactor`, `Remove`, `Upgrade`, etc...  If you're starting with a verb, you're probably in good shape.

    If you're having trouble figuring out how to start the message, make sure your commit message can complete this sentence, "If you load this commit, it will ___________".

- **Avoid the indicative mood on The Summary Line.** The indicative mood is how we speak when we're making statements.  It Git, this frequently results in developers talking about the commit in the past tense.  Don't do it.  You'll look like a rookie.

    ```shell
    # bad - unless you actually like looking like a rookie
    - Added helpers to module X
    - Updated the sign in logic
    - Fixed the sorting bug in user table
    ```

    `Added`, `Fixed`, `Made`, `Refactored`, `Removed`, `Upgraded`, etc...  If you're adding `ed` and `ing` to the end of your verbs, you're on the wrong track.

- **Keep The Blank Line blank.**

- **Separate The Body Paragraph from The Summary Line with ONE Blank Line.**  One. Uno. Unum. Een. Eins. Eyodwa. 'Ekahi. Ceann.  All those words mean exactly **one** somewhere in the world, which, the number of blanks like you should have between The Summary Line and They Body.  Capiche?

- **Limit each line in The Body Paragraph to 72 characters or less.**  Tim Pope argues for a 72 line body in his 2008 [blog post](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).  In a nutshell, this keeps commit messages readable on an 80 character width terminal.

- **Use The Body Paragraph to give more details.**  The Body paragraph is `a possible` place to explain points of interest and known issues to other developers.  Depending on your environment, it may be better to put this information in a Pull Request or issue tracking system where it can receive more attention.

- **Use -m if you want to.**  Many blogs, websites, tutorials and other Git style guides are bullish on always including information in The Body section.  Those guides will tell you not to use the `-m` option when writing your commit message.  GitComm says skip The Body and use `-m` if you want to.  Frequently, background information on a feature is already contained in a separate issue tracking tool, so there's no need to duplicate the information in the actual commit.  If your commits are small, well-defined and easily understandable, it's almost always better to look at the actual changes in the commit itself to understand what happened.  Lastly, it's debatable as to whether or not people even read the The Body in a commit; more people probably look at the text that's written in the Pull Request UI or the issue tracker.

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Commit Structure

- **Branch before you commit.**  You could work directly in `master`, but then you would be certifiably insane (and using the Centralized Workflow).  Create a branch before you start committing.  Branches let you save work, then switch to another task if an emergency task comes up.  Branches let you work on parallel features at the same time (which is very useful when multiple people are working on the project).  Branches let you submit pull requests that can be code reviewed.  Git makes branching cheap; take advantage of it.

- **Restrict commits to a single *unit of change.***  This is similar to the concept behind the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle).  Keep commits focused on a single thought or goal.  Tight, well-defined commits are easier to code review and easier to [cherry pick](https://git-scm.com/docs/git-cherry-pick).  They scream to everyone else that you actually know what you're doing.

- **Keep commits small.**  If the *unit of change* is so massive that it touches 50 files, that unit of change is very likely too big.  Decompose your work into smaller units of change and up your commit count.

- **Relax and let commits break the system when needed.**  At the end of the day, the objective is to create a branch that leaves the project in a better state than when you started. If you need to write commits that aren't quite perfect, or temporarily break the system, man/woman up and do it.  Just make sure everything is top-notch by the time the branch is merged.

- **Tell a story.**  The series of commits should tell a sensible story of how a goal will be achieved by merging your branch into the integration branch.  If your commits can't tell that story in a logical way, why should anyone believe the commits contain quality work?  If you're sloppy with the commits, you're signaling right off the bat that you don't sweat the details.  Another way to say this is **Commit for the Code Reviewer**; commit for the poor SOB who has to actually decipher what you've been working on and determine whether or not it's worth merging.  The more you can make *that guy* happy, the more likely you are to get him to merge your code.

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

## Branches

### Branch Naming

- **Choose a short description for the  name.**  As beautiful as your branch name might be, *Tolstoy*, this isn't the time to write [War and Peace](https://en.wikipedia.org/wiki/War_and_Peace).  **Be brief** so developers have to type less when checking out your branch.

    ```shell
    # good - short and to the point
    refactor-sessions-controller
    ```

- **Use hyphens to separate words.**  A hyphen is the hyphen looking character, not the underscore looking character or the slash looking thing.

    ```shell
    # good
    my-feature

    # bad
    my_feature
    ```

- **Place ticket or issue numbers at the front and separate with a hyphen.**  Other people will likely know the issue number so they can use tab autocomplete to fill in the rest of the branch name easier (numbers also become unique quicker so there can be less to type before autocomplete kicks in).  They're unlikely to know the amazingly creative branch name you chose as a description, so having the number at the end can cause people to do more work when there are lots of branches.

    ```shell
    # good
    15-my-feature

    # bad
    my-feature-15
    15myfeature
    ```

- **DO NOT name a branch with just the issue number.**  Unless they're super on-the-ball, most people won't have a clue what a branch is about if they just see the branch number.  Always add a description so us poor humans can have a little context when we're listing branches.  The branch name will also show up in the commit message when you create a merge commit, so it's helpful to have a little more info in the log that stays around forever.

    ```shell
    # good                  # bad
    15-update-readme        15
    ```

- **DO NOT add tags to the branch name.**  Tags like `bug`, `feature`, `wip` are overkill.   The only thing that matters is that your branch is going to leave the system in an improved state.   Who cares about the *type* of work?  Save the categorization for the issue tracker.

    ```ruby
    # bad
    feature/add-something-cool
    [wip]-refactor-view-html
    ```

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

## Rebasing

Rookies merge.  Masters rebase.

- **Rebase before merging to master or submitting a Pull Request.**  Rebasing makes sure your work will eventually merge without fuss when it's merge time.   More importantly, it brings your code up-to-date before other people start looking at it, or automated tools start running automated checks against it.  Make sure your code works *as if it had been originally written* off the latest and greatest code from your integration branch (i.e. the HEAD of `master`), by first performing a rebase.   THEN submit the Pull Request.  If someone merges to `master` before your Pull Request is finished, rebase again and rerun your integration checks.

- **Rebase frequently.**  Frequent rebasing helps reveal future merge conflicts sooner and it ensures you're developing off the latest functionality from `master` or the integration branch.  This can spare you significant problems if you tend to have longer running feature branches (which is an entirely separate problem...) since you're less likely to have others changing code that you're expecting to be present.

- **Never merge the remote feature branch into a local branch during a rebase.**  Good developers mess this up all the time, so open your mind and prepare to receive some pain saving knowledge.  When you rebase a local branch and then try and push it to the remote, Git *may* try and protect you from yourself.  *If* your branch was previously pushed to the remote server, before the rebase, Git will see two branches with the same name, but with very different commits.  It will block your feeble `git push` attempt and instead, suggest that you merge the remote into your local branch.  **DO NOT DO THIS.**  You intentionally made the branches out of sync by performing a rebase.  Instead, you should **force** push your local branch (via `git push -f`), to overwrite the remote branch history, and get them back in sync.

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Merging

- **Avoid rewriting history on *important branches.***  *Important branches* are branches like `master` or special branches used for releases, integration or the CI server.  These branches are typically used by multiple people and changing history on them, unexpectedly, might get you jacked up.  Even if you're the only person on the project, it's wise to avoid this (there's no need to develop yet *another* bad habit in your life, is there?).  Instead, learn how to use [git revert](https://git-scm.com/docs/git-revert) and fail forward by creating new commits.  Having said all this, we begrudingly acknowledge that there are times when some newb merges 2 weeks of *junk* commits and royally screws up master; sometimes it's just easier to untangle things by resetting the branch.  You gotta do what you gotta do, but it's best to rewrite history as a last resort.

- **Delete a branch from the remote once it has been merged.**  Be a good citizen and cleanup your mess - unless there's a real good reason not to (like you're about the rebase the branch and do some follow-up work on the same issue - or the world's about to end).

- **Rebase before merging to master or submitting a Pull Request.**  Rebasing is your friend.  Learn to master it.

- **Merge frequently.**  Even if your work is perfect, nobody wants to take the time to understand a large set of changes.  It's better to merge frequently, in small steps.  Other developers can digest the work easier and spot potential problems.  You may also need to decompose your user stories or task descriptions so they can worked by smaller branches.

- **Always create a merge commit (i.e. use --no-ff)**  Merge commits give you a place to write information about the entire branch.  They also make it easy for UIs to diff between branches, once they've been merged to `master`, since there is only ever one commit between entire branches.  Merge commits are created by adding the `--no-ff` option to `git merge`.

    ```shell
    # good - used $ git merge 13-update-gems --no-ff
    *    f7cdb99  Closes #13; Merge branch 13-update-gems
    |\
    | *  3382d24  Update rails to '5.1.3'
    | *  4314adf  Update mysql2 to '0.4.9'
    |/
    *    afas43d  Closes #12; Merge branch 12-add-user-sign-in
    |\
    | *  areerag  Styled Forgot My Password page
    ```

    In the above example, UIs will typically automatically diff between single commits so the diff from `afas43d`..`f7cdb99` will be shown since they are neighboring commits (from `master`'s perspective).  In the bottom example, UIs will show `4314adf`..`4314adf` which misses the changes from `areerag`..`4314adf`.

    ```shell
    # bad - did not use $ git merge 13-update-gems
    *  3382d24  Update rails to '5.1.3'
    *  4314adf  Update mysql2 to '0.4.9'
    *  areerag  Styled 'Forgot My Password' page
    ```

- **Squash Before Merging.** Commits in the branch should tell a story.  Squashing commits via `git rebase -i` helps remove unnecessary commits.

- **DO NOT "over squash."**  Some developers go crazy and always squash ALL commits in a branch when they're ready for a merge.  There are situations where this is useful, like when using The Centralized Workflow (described below).  However, as stated in the [Commit Structure](#commit-structure) section above, you want to use your commits to tell a story (which can help others understand your code).  Squash what needs to be squashed and keep what needs to be kept.  Find balance Daniel-san.

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

## Workflows

This section describes some of the well-known Git workflows and describes the trade-offs associated with each one.  All workflows have traits that will make them more, or less, suitable for any given project.   While GitComm holds the position that *there is no perfect workflow*, GitComm is optimized for use with **GitHub Flow**.

1. Avoid adding complexity to a project's Git workflow.  Just because a previous project used one workflow, it doesn't mean the same workflow will be the best choice for the next project.
1. Use the power of rebasing to keep your merge history clean.  This is helpful when others are trying to figure out what has changed.
1. Get out of the *wild west mentality* of integration.  Avoid putting *junk* into the integration branch.  Use automated and manual quality control gates to vet code before it is allowed to be merged.

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### GitHub Flow Workflow (RECOMMENDED)

GitHub Flow is a lightweight workflow that treats `master` as the sole release branch and sole integration branch.  Developers branch off `master` and merge back to `master` once work is complete. There's no need for hotfix branches or other release branches.  Work is simply merged into `master` as it is *ready*.  If problems are found after the fact, a new branch is created and the problem is addressed with the exact same process.

GitHub Flow pairs extremely well with projects using Continual Integration.  It applies the mindset that `master` should always be deployable and highly stable; code should not be merged to `master` unless it passes through quality review controls like code reviews, automated tests and automated code scans.  ([more info](http://scottchacon.com/2011/08/31/github-flow.html))

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Centralized Workflow

The Centralized Workflow is an extremely simply workflow that avoids using branches; it only uses a single `master` branch.  Developers work in their local copy of `master` and then push up to `master` as they reach a checkpoint.   If two developers start working at the same point in `master`, the first developer will get to push up to the remote `master` without any problems.  The second developer will be blocked from pushing to the remote `master` when he tries; instead, he'll have to perform a sync step to update his local master and **rebase** his work (on top of his local `master`) before he can push.

The Centralized Workflow is attractive in many development situations.  It's especially attractive to developers who are new to Git since it tends to limit Git to a workflow that is similar to other SCM tools (i.e. SVN).  It can work well in multi-developer situations, but it doesn't play nice with projects that want to use Pull Requests, or projects that place a strong emphasis on checking code quality before merging to `master`.  ([more info](https://www.atlassian.com/git/tutorials/comparing-workflows))

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Git Flow Workflow

Git Flow is a common workflow that uses multiple branches to handle different concerns of the project.  Git Flow typically uses `master` as a release branch and introduces a second branch called `develop`.  `develop` is treated as an integration branch and developers branch and merge their feature branches off of it, instead of `master`.  Eventually, `develop` reaches a stable point and a release is started.  This involves creating a short lived `release` branch off of `develop`, that can only accept work which helps finalize the upcoming release.  While the `release` branch is accepting cleanup work, developers can continue adding enhancements to `develop`, which shifts towards focusing on a future release.  At some point, the `release` branch is merged to `master`, the release branch is discarded (or repurposed for the next release), and `master` is tagged with a version name. If bugs are found in a tagged release in `master`, developers can create a follow-up release (if `develop` hasn't advanced *too far*) or introduce a hotfix branch that immediately addresses the specific issue.

Git Flow excels on projects where development on the current release needs time to wrap-up, while development on the next release  starts.  Introducing a `develop` branch is also useful when developers need more freedom to integrate with less oversight, or features need to be bundled up and reviewed as a group before being merged to `master`.  However, this extra capability adds significant complexity to the workflow, and the benefits of Git Flow can often be realized by changing the development/release process to something that is more lean (like [GitHub Flow](#github-flow-workflow-recommended) of course).  ([more info](http://nvie.com/posts/a-successful-git-branching-model/))

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Integration-Manager Workflow

The Integration-Manager Workflow is the standard fork and pull request workflow that is used by most open source projects.  In this workflow, one or more developers maintain the primary repository.  Other developers can copy (i.e. *fork*) the primary repository and work in their own copy.  At some point, they can submit a pull request to the primary repository, asking that one of their branches be merged into the primary repository.  At that point, the developers who have control over the primary repository can accept/reject the pull request, or ask for modifications.

This workflow is highly useful for development teams that aren't directly connected.  It allows other developers to contribute to projects while maintaining control over the code base.  This workflow is usually paired with one or more other workflows as it doesn't dictate how developers should operate within their own copy of the repository. ([more info](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows))

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)

### Dictator and Lieutenants Workflow

The Dictator and Lieutenants Workflow is essentially the Integration-Manager Workflow on steroids.  It introduces other repositories, each with its own integration manager, that can help process pull requests for different pieces of the overall project.  Developers contributing to the project submit pull requests to a lieutenant's repository; once the lieutenant's have integrated enough pull requests, they submit a pull request to the dictator, who updates the primary repository.

This type of workflow is very useful for large projects where there are just too many contributors for one or two people to effectively review all the incoming code.   Dictator and Lieutenants enlists other people to help review and merge, without giving them write access to the main repository.  The Linux kernel is frequently cited as a project that uses this type of workflow.  ([more info](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows))

[Back to the top](#gitcomm---a-guide-to-git-user-happiness)
