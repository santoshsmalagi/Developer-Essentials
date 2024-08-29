# Git

## Cloning a repository

Start by creating a personal access token:

Create Personal Access Token on GitHub
From your GitHub account, go to Settings → Developer Settings → Personal Access Token → Tokens (classic) → Generate New Token (Give your password) → Fillup the form → click Generate token → Copy the generated Token, it will be something like ghp_sFhFsSHhTzMDreGRLjmks4Tzuzgthdvfsrta

For Linux, you need to configure the local GIT client with a username and email address,

```bash
$ git config --global user.name "your_github_username"
$ git config --global user.email "your_github_email"
$ git config -l
```

Once GIT is configured, we can begin using it to access GitHub. Example:

```bash
$ git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY
> Cloning into `YOUR-REPOSITORY`...
Username: <type your username>
Password: <type your password or personal access token (GitHub)
```

Now cache the given record in your computer to remembers the token:

```bash
$ git config --global credential.helper cache
```

If needed, anytime you can delete the cache record by:

```bash
$ git config --global --unset credential.helper
$ git config --system --unset credential.helper
```

Now try to pull with ``-v`` to verify

```bash
$ git pull -v
```

Linux/Debian (Clone as follows):
```bash
git clone https://<tokenhere>@github.com/<user>/<repo>.git
```
