# How to Protect the `main` Branch

This document explains how to configure branch protection rules for the `main` branch in a GitHub repository, using the [EditorConfig Checker GitHub Action](https://github.com/marketplace/actions/editorconfig-checker-action).

## Set Up EditorConfig Checker Action

First, create a GitHub Actions workflow configuration directory:

```sh
mkdir -p .github/workflows
```

Next, copy the YAML configuration from the [EditorConfig Checker Action page](https://github.com/marketplace/actions/editorconfig-checker-action) and paste it into a new file:

```sh
pbpaste > .github/workflows/editorconfig-checker.yml
```

> [!NOTE]
> `pbpaste` is a macOS command. On Windows or Linux, manually paste the configuration into a new file using your preferred text editor.

Create a `.editorconfig` file in the root of the repository with the following content:

```ini
root = true
[*]
insert_final_newline = true
```

- [EditorConfig-Properties#insert_final_newline](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties#insert_final_newline)

Commit and push the changes to the `main` branch:

```sh
git add .
git commit -m "Set up EditorConfig Checker"
git push origin main
```

### Verify the Action

Create a test branch and add a file that intentionally violates the `.editorconfig` rules:

```sh
git switch -c sample

# Create a file without a trailing newline
printf "%s" aaa > a.txt

git add a.txt
git commit -m "Add file with EditorConfig violation"
git push -u origin sample
```

Create a pull request. If you have the GitHub CLI installed, you can use the following command:

```sh
gh pr create --base main --title "Sample Pull Request"
```

Alternatively, you can create the pull request through the GitHub web interface.

Once the pull request is created, verify that the GitHub Actions workflow runs and fails due to the missing trailing newline in `a.txt`.

## Add a Rule to Protect the `main` Branch

Set up a branch protection rule via the repository settings:

1. Open your GitHub repository in the browser.
2. Go to **Settings** > **Branches** > **Branch protection rules**.
3. Click **Add rule**.
4. Configure the rule with the following settings:
   - **Branch name pattern:** `main`
   - ✅ **Require a pull request before merging**
   - ✅ **Require status checks to pass before merging**
     - From **Add checks**, search for and select `editorconfig (GitHub Actions)`
5. Save the rule to enable protection.

> [!NOTE]
> It appears that a status check name will only show up in the list after the workflow has run at least once.

### Verify the Ruleset

With the rule in place, all pull requests to the `main` branch must pass the status check from the EditorConfig Checker before they can be merged.

## Summary

- The `main` branch is now protected with the following rules:
  - Pull requests are required for all changes
  - Status checks must pass before merging

    - Specifically, the `editorconfig (GitHub Actions)` check
- The repository includes a `.editorconfig` file with this rule:
  - All files must end with a newline (`insert_final_newline = true`).

🔒 This setup ensures consistency in file formatting by enforcing trailing newlines for all files merged into `main`.

## Resources

- [GitHub CLI](https://cli.github.com/)
- [EditorConfig Checker Action](https://github.com/marketplace/actions/editorconfig-checker-action)
- [EditorConfig](https://editorconfig.org/)
