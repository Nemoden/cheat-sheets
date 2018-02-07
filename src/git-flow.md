# Git-Flow

## Initialize a Repository for git-flow

    git flow init -d

(Omit `-d` if you want to select values other than the defaults.)

## Features

### Start a New Feature

This creates a new branch based on `develop` and switches to it:

    git flow feature start FEATURENAME

### Publish a Feature

Push a feature branch to remote repository:

    git flow feature publish FEATURENAME

Get a feature published by another user from remote repository:

    git flow feature pull origin FEATURENAME

### Finish a Feature

This merges the feature into `develop`, removes the feature branch, and switches to `develop`:

    git flow feature finish FEATURENAME

## Releases

### Start a Release

Create release branch from `develop`:

    git flow release start RELEASENAME

Publish release branch:

    git flow release publish RELEASENAME

Create a local tracking branch for a remote release:

    git flow release track RELEASENAME

### Finish a Release

Merge release branch into `master`, tag it, merge back into `develop`, and remove the release branch:

    git flow release finish RELEASENAME
    git push --tags

## Hotfixes

### Start a Hotfix

Create hotfix branch from `master`:

    git flow hotfix start VERSIONNAME

Create hotfix branch from some other commit:

    git flow hotfix start VERSIONNAME BASENAME

### Finish a Hotfix

Merge hotfix back into develop and master, and tag:

    git flow hotfix finish VERSIONNAME
