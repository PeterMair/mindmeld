# MindMeld Workbench 3.0

[![CircleCI](https://circleci.com/gh/expectlabs/mindmeld-workbench3.svg?style=svg&circle-token=437cf905895688ac1b58b60fe79144c180893372)](https://circleci.com/gh/expectlabs/mindmeld-workbench3)

This repository contains documentation for MindMeld Workbench 3.0.

## TLDR: Quick Start

Assuming you have pyenv installed.
```
cd mindmeld-workbench3/  # go to the root of the workbench repo
pyenv install 3.5.3  # install an appropriate version of python
pyenv virtualenv 3.5.3 workbench3  # create a virtualenv
pyenv local workbench3  # set the virtualenv for the repo
pip install -r dev-requirements.txt  # install development dependencies
```

## Getting Started

Workbench 3.x is developed primarily in Python 3.4+, but has support for Python 2.7 as well. Best practice is to set up a virtualenv using the latest stable version of Python for Workbench development. Steps below describe how to do this, assuming pyenv is installed on your system. If not see (pyenv on the eng-wiki)[https://github.com/expectlabs/eng-wiki/wiki/pyenv].

Make sure you're in the root of the repo
`cd mindmeld-workbench3/`

Install an appropriate version of Python, 3.5.3 or 3.6.0
`pyenv install 3.5.3`

Create virtualenv with that Python for Workbench 3
`pyenv virtualenv 3.5.3 workbench3`

Set that virtualenv to be automatically activated for the repo.
`pyenv local workbench3`

Install the development dependencies.
`pip install -r dev-requirements.txt`

## Building Docs

`make docs`

(You might see a warning about mallard.rst. Safe to ignore that for now.)

## Viewing Docs

`open docs/build/html/index.html`

## Releasing a Version to MindMeld's PyPI

**Beware! Make sure you read this guide and know what you are doing!**

### Bump the Version

We use a python utility for bumping the version, aptly named `bumpversion`.

This utility will update the version in all the places it should, create a new commit `Bump version: {old-version} → {new-version}`, and create a tag for the new version.

Our versioning format is `{major}.{minor}.{patch}{release}{revision}`. So version part should be any of `major`, `minor`, `patch` `release` or `revision`.

Let's say the current version is `3.0.1dev2`. `bumpversion revision` would bump the version to `3.0.1dev3`. 

If this is your first release, it is recommended that you do a dry run first to confirm you are using the correct version part:

```
bumpversion <version-part> --dry-run --verbose
```

#### TLDR; Do The Thing

```
# Bump the Version
bumpversion <version-part> --commit --tag
git push
git push --tags
```

### Building the wheel

Python wheels are the preferred binary package format for python packages. If you care to learn more, read [here](http://pythonwheels.com/) or [here](https://www.python.org/dev/peps/pep-0427/)

#### TLDR; Do The Thing

```
python setup.py bdist_wheel --universal
```


### Publishing to PyPI

The process for publishing to PyPI is still quite rough. For now it involves manually copying the package archive to our PyPI server. This process will need to be repeated for each environment. The PyPI servers in each environment are: `dev-pypi-000.dev`, `staging-pypi-000.staging`, and `prod-pypi-000.prod`

#### TLDR; Do The Thing

```
# Confirm you are connected to MindMeld VPN
scp dist/mmworkbench-{new-version}-py2.py3-none-any.whl dev-pypi-000.dev:~/
ssh dev-pypi-000.dev

# Wait for ssh connection to complete
sudo su -
mv /home/{your-username}/mmworkbench-{new-version}-py2.py3-none-any.whl /opt/pypi
chown root:root /opt/pypi/mmworkbench-{new-version}-py2.py3-none-any.whl
```
