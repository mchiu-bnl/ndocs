## Introduction to Git<a name="introduction"></a>

### VCS<a name="intro-vcs"></a>

VCS stands for [Version Control System](https://en.wikipedia.org/wiki/Version_control).

> In computer software engineering, revision control is any kind of practice that
> tracks and provides control over changes to source code. Software developers
> sometimes use revision control software to maintain documentation and
> configuration files as well as source code. (C) Wikipedia

The need to logically organize work with documents (various types of) existed
for a long time. But it was solved only in 1980's by introduction of
computer-based office and VCSs. Now version control is broadly used in work
with text documents. It allows to control revisions, keep history, see changes
and track authorship. This is very good for housekeeping, when a team of people
works concurrently. And since source code is usually plain text, it was found to
be extremely useful in programming. A good description of benefits is
[here](https://www.atlassian.com/git/tutorials/what-is-version-control).

Thus in nEXO we have a strong wish that every (important) piece of code or
documentation should be under version control. This means, that only code from
official repository is allowed to process and analyze data. Any small or side
analysis projects, that doesn't go to main line, but which results are used in
some way, should be stored in 'User area' repository. It is recommended that
source files of technical notes are also kept in repository.

### Git<a name="intro-git"></a>

A popular software for version control is [Git](https://en.wikipedia.org/wiki/Git).

> Git is a distributed version-control system for tracking changes in source
> code during software development. It was created by Linus Torvalds in 2005
> for development of the Linux kernel. (C) Wikipedia

Distinguished features of Git are:

- distributed development (every Git directory if a full-fledged repository)
- non-linear development (strong support of branching and merging)
- efficient handling of large projects (very fast and scalable)
- toolkit-based design (a set of component that are easy to chain together)
- good model of merge strategies with several powerful algorithms
- automatic garbage collection from repository database

A bunch of arguments in favor of Git is [here](https://www.atlassian.com/git/tutorials/why-git).

Git is an open-source software available for most systems ([see](https://git-scm.com/downloads)).
It was created as a set of command-line tools. So to have all options and see
the full power, one has to use command line. Use `git help` or `man git`
commands for usage information. There are also many GUI applications for Git,
both free and commercial ([see](https://git-scm.com/downloads/guis)).

An extensive git manual can be found [online](https://git-scm.com/docs) or in
'Pro Git' book available online or printed.

### GitHub<a name="intro-github"></a>

[GitHub](https://github.com/about) is a web-based hosting service for version
control using Git. GitHub also provides access control, plus also collaboration
functions such as bug tracking, feature requests, task management, and wikis
for every project. The objective of GitHub, and what it is marketed as, is to
simply promote collaborations between developers, allowing them to get a new
set of ideas on the project.

Projects on GitHub can be accessed and manipulated using the standard Git
command-line interface and all of the standard Git commands work with it.
GitHub also allows registered and non-registered users to browse public
repositories on the site. Multiple desktop clients and Git plugins have also
been created by GitHub and other third parties that integrate with the platform.

GitHub provides a powerful [documentation site](https://help.github.com/) with
manuals describing all available functions and work patterns.

[About Git and GitHub](https://medium.com/@eduoshaun/difference-between-git-and-github-807f1a57d438)
