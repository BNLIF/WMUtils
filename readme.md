Notes Regarding Usage on SDCC
==================

Date: 2022.02.22

This repository was originally forked from https://github.com/AIT-WATCHMAN/WMUtils in Feb 2022. The `watchmanInstaller.sh` will download a fresh copy of cmake, python, geant4, ratpac, ... and build them from scratch. The installation will result a 10 GB WMUtils directory containing all neccessary dependences. Some are useful for WbLS, some are not. Thus, we do not want to do multiple installations on the same machine (ex. dune0001.sdcc.bnl.gov). Instead, we have one shared group directory at /gpfs01/lbne/users/wbls where this WMUtils is installed. People are encouraged to develop ratpac in their own home directory under /gpfs01/lbne/users, and compile ratpac on their own by linking the dependence in this WMUtils. A separate ratpac repository is forked into BNLIF/WbLS as well, and we should be using that one to corrodinate separate code development.

The original watchmanInstaller.sh does not work out of the box, so we made the following two changes:  
     - libOpenGL -> libGL
     - root v6.18.00 -> v6.18.00-patches
The changes has been updated to this repository. If further edits are required (either for fixing issue, installing on other machine, or develop analysis code), we can modify this repo as we want.

We will keep this repo public until people request privacy.

Watchman Utilities
==================
**Maintainer:** [@MorganAskins](https://github.com/MorganAskins)
<div align="center"><img src="wmutils.svg" width=50% /></div>

This repository is an organized dumping ground for watchman related code which
should include useful tools (such as watchmakers and sibyl), related analysis
scripts, macro files, and anything else that does not belong directly in
rat-pac.

watchmanInstaller.sh
--------------------
The watchman installer script is designed to help with the installation of
various components of the watchman software suite. It is not designed to
be completely fool-proof or work on every system. Compatibility seems to
be best with various newer versions of linux. Each piece of the package
can either be installed separately or all together; for a first time through
I would recommend installing and testing each piece in order.

To run:
```bash
./watchmanInstaller.sh
```

To skip individual components, or target specific components use
```bash
./watchmanInstaller.sh --skip $X
## OR
./watchmanInstaller.sh --only $X

── $X ∈ (cmake, python, root, geant, ratpac, sibyl)
```

The installer will check if particular libraries are already available on
the system before running. If they are not it is recommended to go install
them through your system's package manager. If the check fails but you want
to attempt to install everything anyways, then you can skip the checks
```bash
./watchmanInstaller.sh --skip-checks
```

Finally, multi-thread the compilation, you can pass through the number of
processors for cmake to use
```bash
./watchmanInstaller.sh -jN
```

Once complete, everything should be installed locally and a file `env.sh`
will be created---source this to setup the working environment.

### Example: Ubuntu 20.04
```bash
sudo apt-get -q update
sudo apt-get install git curl build-essential libx11-dev libxpm-dev \
  libqt5opengl5-dev xserver-xorg-video-intel libxft-dev libxext-dev \
  libxkbcommon-x11-dev libopengl-dev libcurl4-gnutls-dev gfortran \
  ca-certificates libssl-dev libffi-dev

./watchmanInstaller.sh
```

Using your own ratpac
---------------------
You can work in this directory and everything that starts with ratpac or
rat-pac is already in the .gitignore. To pull in your own version of rat-pac
for development the following is recommended (assuming ssh key). Another
recommendation would be to keep every development branch in its own folder.
```bash
git clone git@github.com:Username/rat-pac.git ratpac_development
cd ratpac_development
# Add ait-watchman as a remote and update your master branch accordingly
git remote add upstream git@github.com:AIT-Watchman/rat-pac.git
git fetch upstream
git merge upstream/master
git push origin master
```
Development should occur on your repository on an aptly named branch. A good
choice would be to give the branch the same name as the folder it is being
developed in.
```bash
git checkout -b development
# Make changes to the code
git add changed_files
git commit -m
git push origin development
```
Finally if you are reviewing another user's pull request, you can directly
fetch the pull request by number. E.g. if I want to checkout pull-request #1234:
```bash
git clone git@github.com:ait-watchman/rat-pac.git ratpac_devpr
cd ratpac_devpr
git fetch origin +refs/pull/1234/merge
git checkout FETCH_HEAD
```

