# Quick Start 2
In [Quick Start](quickstart.md), we learn how to setup and use the software if all the software are installed under your own directory.
But actually, we don't rebuild the external libraries every time. In this part, we will learn how to setup a developing environment when there is an official release.

Assume you are already on SLAC login node, and you also could access an existing release (`$NEXOTOP`):
```
/nfs/slac/g/exo_data4/users/ljwen/nexo-sw/nexo-june27
```
## create a new developing environment
First, create your own `$WORKTOP` for your own developing:
```bash
$ export WORKTOP=/nfs/slac/g/exo_data4/users/ljwen/nexo-sw/nexo-dev
$ mkdir -p $WORKTOP
```

Then, we need to prepare a setup script. For example, create a script called `bashrc` (for bash user) under `$WORKTOP`:
```bash
export NEXOTOP=/nfs/slac/g/exo_data4/users/ljwen/nexo-sw/nexo-june27
export WORKTOP=/nfs/slac/g/exo_data4/users/ljwen/nexo-sw/nexo-dev
export NEXO_OFFLINE_OFF=1 # disable offical nexo-offline

source $NEXOTOP/setup.sh # reuse the external libraries, nexo-ei, nexo-sniper

export CMTPROJECTPATH=$WORKTOP:$CMTPROJECTPATH # prepend $WORKTOP, so that CMT looks up in $WORKTOP first.

pushd $WORKTOP
  if [ -d "$WORKTOP/nexo-offline" ]; then
    pushd nexo-offline/nEXORelease/cmt/ >& /dev/null
    source setup.sh
    popd >& /dev/null
  fi
popd
```

Source the `bashrc` file:
```bash
$ source bashrc
```

Now, it is time to clone the code:
```bash
$ git clone git@github.com:nEXO-collaboration/nexo-offline.git
```
Note, if you don't have permission to access the repository, please make sure:
* Your github account can access nEXO's software
* Your ssh public key is uploaded to github

Compile the software:
```bash
$ cd nexo-offline/nEXORelease/cmt/
$ cmt br cmt config
$ source setup.sh
$ cmt br cmt make  
```

## Setup your existing developing environment
If your developing environment is ready, the next time what you need to do is `source bashrc`.
