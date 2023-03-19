# Python setup and installation guide

## Description:

---

The purpose of this guide is to help the readers understand the importance of setting up python properly and also how to install python on their local devices.

> The prerequisite for setting up on macos is to have homebrew installed and for windows, to have wsl installed. wsl on windows allows us to work in a linux environment which is much easier.

## Why do we have to setup python properly?

---

On some systems like macos, python comes preinstalled. Some parts of the system may depend on this python and hence we should not mess it up.

To setup our python properly, we need to take care of 2 things.

1. To be able to switch between different python versions.
   > (Due to requirements like your different projects requiring different python versions.)
2. To be able to have different environments for each individual project.
   > (Some projects may require different version of pip packages but the same python version.)

\
When you search on google, you will see many different terminologies and recommendations on how to setup python. Some solutions may work but may be troublesome / not easy to use.

> The following terminologies are all referring to distinct and different objects:
>
> - pyenv
> - virtualenv
> - venv
> - virtualenvwrapper
> - pyenv-virtualenv
> - pyenv-virtualenvwrapper
> - virtual environment (a term used to say an environment that is not the main global python environment. these virtual ones can be easily deleted and recreated again.)

\
Dilemma:  
_pyenv_ is the most commonly used python version manager which allows us to change and switch between different python versions. _virtualenv_ and _venv_ both does the same job of creating virtual environments. _venv_ is the successor to _virtualenv_ and comes together by default with python3 (>3.3). however, note that _venv_ is a subset of _virtualenv_. use _virtualenv_ if we need to work with python2 for backwards compatibility.

to manage environments created using _virtualenv_, we usually use _virtualenvwrapper_ to make things easier. if we just use _virtualenv_ by itself, we have to always manually find the virtual environments located in different places just to activate/deactivate them, but _virtualenvwrapper_ helps us to simplify things but allowing us to access them "all in one place". (_virtualenvwrapper_ can also be used on top of _venv_, just need to do some modifications.) _virtualenvwrapper_ is installed for every different version of python. (NOT the base system python, but the individual python versions installed via _pyenv_).

This is troublesome and that is where _pyenv-virtualenvwrapper_ comes into play. it helps with handling the "installing _virtualenvwrapper_ on every python version" part. however, looking back, we dont even need to install _virtualenvwrapper_ in the first place if all our virtual environments are not all over the place. by using _pyenv-virtualenv_, actually all our virtual environments are already "grouped together", hence we dont have to use *virtualenvwrapper*or _pyenv-virtualenvwrapper_ even.

_pyenv-virtualenv_ will automatically use _virtualenv_ or _venv_ depending on the python version. if want to use the most low level, then just use _virtualenv_ / _venv_ individually with _pyenv_. but using _virtualenv_ and _venv_ individually will be a little more troublesome in that the environments are not in the same place. as mentioned above, _virtualenvwrapper_ is usually used to solve this problem.

Another alternative if want to make things easy, can be to use _anaconda_ but note that conda has a distinct / different package repo from pypi. in some cases may affect.

## How to setup python properly once and for all.

---

As mentioned above, we just need to be able to handle that 2 conditions. I **recommend using just _pyenv_ and the _pyenv-virtualenv_ plugin**. pyenv is for allowing us to download and switch between python versions. pyenv-virtualenv allows us to create the different python virtual environments which can be used for different projects.

### Bird's eye view on how a python project is usually managed and how the different components come together:

First we have our operating system, such as macos, windows, linux. To install packages on the system level, we need a package manager. For macos, we usually use homebrew, and for linux, we usually use apt.

Using brew, we can install pyenv and pyenv-virtualenv at the os level. We will then use pyenv to install the different python versions that we want to use. when these python versions are installed, they come with their own pip, which is a package manager to install packages at the python level. this is different from the package manger at the os level (like brew and apt). Usually, we call these the global python and global pip.  
Next, we have pyenv-virtualenv. pyenv-virtualenv is a plugin to pyenv and it allows us to create virtual environments for different projects that we are working on.

So what is a virtual environment? In simple terms, it is an isolated python environment. What it means is that, for example, if we create a virtual environment from python 3.9, that virtual environment will have its own pip (different from the global one), and any python packages that you install via this pip will only be for this virtual environment. It will not be installed into the global pip packages directory. how this is useful is that, if we have 2 virtual environments that require the same python version, but different version of a same pip package, there may be a conflict if we just install using the global pip. If we look at venv2 and venv3 in the following diagram, we can see that they both depend on dependency3, but venv2 requires version1 and venv3 requires version2. If we use the global pip to install, the version that was installed the latest would override the previous version. But virtual environments allow us to have a separate pip manager and hence different and isolated package installations.

Now you may be wondering what are pip packages and where are they installed from? pip packages are just libraries / code that other people have written that can be readily used in your code by importing them. Hence you dont have to write the code yourself, but can just use the functionality. These packages are uploaded by other developers into a public repository (called the python package index) and that is where pip installed your python packages from. You can think of the repository as a online cloud like onedrive, where you can download files and folders that other people has uploaded.

## Summary of how I use it

---

- pyenv to manage different python installation versions. installed on system level (apt / homebrew).
- pip ("pip" command) to install packages for a certain python installation. installed on python version level. (already comes together when install any python version using pyenv)
- pyenv-virtualenv to create virtual environments (either venv or virtualenv). installed on system level. (this plugin comes together when installing pyenv via installer)
- consider: using pipx to install pip packages that you need for all virtualenvs, instead of duplicating installations.

## _pyenv_ commands

---

### The benefit to this usage is that everything "belongs to" pyenv and i just need to know the following commands.

```zsh
# https://github.com/pyenv/pyenv/blob/master/COMMANDS.md
pyenv global
pyenv versions
pyenv version
pyenv install --list
pyenv install/uninstall <version>
pyenv global <version>
pyenv virtualenvs
pyenv virtualenv <version> <virtual env name> # (use this for more clarity)
pyenv virtualenv <virtual env name> # (uses the current selected pyenv global)
pyenv uninstall <virtual env name>
pyenv activate <virtual env name>
pyenv deactivate
```

## Additional notes

---

- it is common to name environments after the project
- the purpose of pyenv local is in case u forget to activate virtual environment for your project and accidentally install on global python. global python here refers to a single python version which is not in a virtual environment. eval "$(pyenv virtualenv-init -)" this line of code in .zshrc helps us to automatically activate directories with pyenv local. however, this makes the CLI slow.
- it is recommended to use "python -m pip install package"
- system python will refer to the python installation that comes with the OS.
