# ci-sample

This repository is used to test the [editorconfig-checker-action](https://github.com/marketplace/actions/editorconfig-checker-action) GitHub Action.

## Purpose

The `editorconfig-checker-action` verifies that all files in the repository comply with the rules defined in the `.editorconfig` file, and reports any violations.

## How It Works

* The GitHub Action is referenced from the `main` branch.
* The `.editorconfig` file used for validation is the one from the *target branch* (i.e., the branch being checked in the pull request).
