## Quick guide how to commit changes

This is a common sequence of actions to introduce new changes to the source code
of the project, whether it is a bug fix, update or improvement. It is intentionally
very dry.

#### 0. Preparation

Read or be familiar with the [detailed description](UsingGit.md). Prepare a
system you'll be working with, either using [Docker image](docker.md) or
[native install](quickstart.md).

#### 1. Create new branch ([details](UsingGit.md#branchflow))

Open the [project](https://github.com/nEXO-collaboration/nexo-offline) page in
browser, switch to 'master' branch, then
[create](https://help.github.com/en/articles/creating-and-deleting-branches-within-your-repository)
a new branch for you. Choose the branch name carefully, follow common style.

#### 2. Prepare local repository ([details](UsingGit.md#repositories))

You can create an absolutely new copy:

```bash
$ git clone https://<user_name>@github.com/nEXO-collaboration/nexo-offline.git
$ cd nexo-offline
```

or update already existing one:

```bash
$ cd nexo-offline
$ git checkout master
$ git pull origin
```

#### 3. Introduce your changes ([details](UsingGit.md#making_changes))

In our example we will update the `Doc/Readme.md` file:

```bash
$ git checkout --track origin/<your_branch>
$ pico Doc/README.md
```

Actually you can use any editor you prefer, `pico` is only an example.

#### 4. Check that code compiles ([details](quickstart2.md))

If you're using CMT:

```bash
$ cd nEXORelease/cmt
$ cmt br cmt make
$ cd ../..
```

or if you prefer CMake:

```bash
$ cd build
$ make
$ cd ..
```

#### 5. Check that the code works

Run your script to see that it works. Also run a generic example to check that
you didn't break things.

#### 6. Check and commit your changes ([details](UsingGit.md#commits))

```bash
$ git status
$ git add Doc/README.md
$ git diff --cached
$ git commit
```

Avoid unnecessary changes. Write a thoughtful commit message. Follow
[recommendations](UsingGit.md#commit_message). If you fix a bug, please
reference [Issue](https://github.com/nEXO-collaboration/nexo-offline/issues).

#### 7. Upload to remote ([details](UsingGit.md#remote_branches))

```bash
$ git push origin <your_branch>
```

#### 8. Create Pull Request ([details](UsingGit.md#pull_request))

Open the [project](https://github.com/nEXO-collaboration/nexo-offline) page in
browser and [create](https://help.github.com/en/articles/creating-a-pull-request)
a new Pull Request from your branch. Write a good description of your changes and
intentions, pointing to any pertinent GitHub issues or slides presented at meetings.

#### 9. Follow the discussion

Follow your [Pull Request](https://github.com/nEXO-collaboration/nexo-offline/pulls).
There will be a [Code Review](UsingGit.md#code_review). Keep in touch with this,
be ready to fix things if asked. Don't miss when it will be merged to 'master'
branch.

#### 10. Delete the branch once merge to master is complete

Once your updated code is merged to master, it is time to consider deleting your branch.  We do not have automatic branch deletion turned on in our GitHub repositories.  It is up to us to remove branches manually.  Once you are satisfied that your changes are in master, follow the instructions in the [GitHub documentation](https://help.github.com/en/articles/creating-and-deleting-branches-within-your-repository#deleting-a-branch) to remove your branch.
