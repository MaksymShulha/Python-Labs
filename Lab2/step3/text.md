# Working with a Git Repo

It is important to understand that the lab environment is temporary. It only lives for a short while before it is destroyed. It would be best if you pushed all changes made to your own GitHub repository to recreate it in a new lab environment when required.

Also, note that this environment is shared and, therefore, not secure. You should not store personal information, usernames, passwords, or access tokens in this environment for any purpose.

To protect yourself from re-work, you must occasionally commit and push your code to a GitHub repository.


## Review changes

To review the changes that have been made, run the following commands in the terminal:

```bash
cd [your repo name]
git status
```

## Mark changes for commit

You now need to commit the changes you\'ve made. Before you can do that, you need to add the new and revised files to the commit:

```bash
git add sample.html
git add existing_file.html
```

After adding the files, rerun `git status`.

## Git setup - Your identity

The first thing you should do when you start using Git is to set your user name and email address. This is important because every Git commit uses this information, and it will be part of the commits you start creating. Replace the given Username and Email ID with your personal credentials:

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

## Commit the changes

You are now able to commit the changes you\'ve made. Run the following command to commit the changes. You will pass a commit message using the `-m` option.

```bash
git commit -m 'changes made from the lab environment'
```

## Git Remote

1. Create a free account on [GitHub](https://github.com/ "GitHub")

1. Verify your email address, if you haven\'t done so already

1. Go to Settings: In the upper-right corner of any page, click your profile photo, then click Settings

	![Settings](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/settings.png "Settings")

1. Go to Developer Settings

	![Developer Settings](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/developer-settings.png "Developer Settings")

1. Create personal access token to authenticate to GitHub from your environment

	![Create personal access token](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/generate-new-token.png "Create personal access token")

1. Set privileges for the token and generate

	![New token permissions](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/token-permissions.png "New token permissions")

	![Generate Token](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/create-token.png "Generate Token")

1. Create a remote repository to push your code

	![New repository menu](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/new-repository.png "New repository menu")

	![Create Repository](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/create-new-repository.png "Create Repository")

1. When you clone a repository with git clone, it automatically creates a remote connection called origin, which points back to the cloned repository. This is useful for developers creating a local copy of a central repository since it provides an easy way to pull upstream changes or publish local commits.

	If the remote origin is already set (most likely to happen when you clone from GitHub).
	Replace `YOUR_GITHUB_USER` in the following commands with your actual GitHub username.
	```bash
	git remote set-url origin https://github.com/YOUR_GITHUB_USER/my-course-repo.git
	```
	Creating remote origin the first time (when you start with a blank repository locally).

	```bash
	git remote add origin https://github.com/YOUR_GITHUB_USER/my-course-repo.git
	```

1. You will be presented with multiple options for adding code to this repository. In your lab environments, you will be provided boilerplate code, so the best option is to **push an existing repository from the command line**.

	![push existing repository](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0211EN-edX/images/import-commands.png "push existing repository")

	While running the below commands, you will be prompted to enter a `username`. This will be your GitHub username. And the `password` will be the access token that you generated earlier.

	Then:

	```bash
	git branch -M main
	git push -u origin main
	```
