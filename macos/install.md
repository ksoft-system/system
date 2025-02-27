# MacOS setup steps

## Prerequisites

- sign-in/sign-up Apple ID
- update to the latest MacOS version

## Apps

### System settings

- personalize basic system settings

### Widgets

- use Desktop widgets instead of Notifications Center widgets because of Desktop/Documents focused workflow

### Dock

- remove all applications besides the permanent Finder and Trash
- automatically hide the dock

### Finder

- delete tags
- show path bar
- personalize sidebar
- personalize toolbar
- personalize [default view options](https://support.apple.com/en-uz/guide/mac-help/mchldaafb302/mac)
- personalize Desktop view options
- personalize Applications folder view options
- personalize Downloads folder view options

### Launchpad

Organize apps on 2 pages:  
1. Apple apps
2. Other apps

> Use the most sensible order of apps, so they relate to eachother

### Safari

### Reminders

### Notes

### Activity Monitor

### Terminal

### Photos

#### Configure automatic screenshot saving into Photos app

1. Create folder `~/Pictures/Screenshots`
2. [Create Automator Folder Action](https://www.quora.com/Can-I-make-my-macOS-screenshots-automatically-save-to-the-Photos-app-instead-of-the-desktop) named `Import Screenshots to Photos` using that folder
3. [Set shouldDeleteSourceFiles to True](https://discussions.apple.com/thread/7618522?sortBy=rank)
4. Set Screenshot tool's save location to the newly created `~/Pictures/Screenshots` folder

## App Store

### Install apps which are generally unavailable through homebrew

Apple apps

- Numbers
- Pages
- Keynote
- iMovie
- GarageBand

Development

- Xcode

Other

- Kindle

> In the future, consider using `mas` to automatically install these apps from Brew Bundle. The support seems to be broken as of now.

## Development

### Sharing

Change computer names:

```
sudo scutil --set ComputerName YOUR_NAME
sudo scutil --set LocalHostName YOUR_NAME
sudo scutil --set HostName YOUR_NAME
```

### Directories

Inside home folder create a new folder for all development tasks:

```
mkdir ~/Developer
```

### Package management

Install Homebrew package manager:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Use Homebrew to install all software:

```
brew install --formulae (CLI tools Linux/GNU)
brew install --cask (GUI .app official MacOS applications)
```

Save Homebrew update command:

```
brew update -v && brew upgrade -v && brew autoremove -v && brew cleanup -s --prune=all -v
```

### Shell environment

[Change default shell from Zsh to Bash.](https://stackoverflow.com/questions/77052638/changing-default-shell-from-zsh-to-bash-on-macos-catalina-and-beyond)  
Remove default Zsh files:

```
rm -r ~/.zprofile ~/.zsh_history ~/.zsh_sessions
```

### [SSH Authentication key - GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Generate ssh key:

```
ssh-keygen -t ed25519 -C "kk-ssh"
eval "$(ssh-agent -s)"
touch ~/.ssh/config
```

Also see [ssh key naming explanation](https://stackoverflow.com/questions/72626602/what-shall-i-use-as-a-comment-while-creating-the-ssh-key-and-how-this-comment-wi).

Edit ~/.ssh/config file, then modify the file to contain the following lines:

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/kk-ssh
```

Add private key to the ssh-agent:

```
ssh-add --apple-use-keychain ~/.ssh/kk-ssh
```

Copy public key to your clipboard:

```
pbcopy < ~/.ssh/kk-ssh.pub
```

Add the copied public key to the ssh authentication keys in GitHub settings.

Test your ssh connection:

```
ssh -T git@github.com
```

Enter one of [GitHub's public key fingerprint](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints):

```
SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
```

From now on, your ssh agent should add GitHub public key fingerprints to your ~/.ssh/known_hosts file to avoid manually verifying GitHub hosts.

### SSH Signing key - GitHub

Add ssh public key from steps above to the signing ssh keys in GitHub page.

Tell git about your ssh key to sign offline git commits:

```
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/kk-ssh.pub
git config --global commit.gpgsign true
```

### [Install nvm](https://formulae.brew.sh/formula/nvm)

```
brew install nvm
mkdir ~/.nvm
echo 'export NVM_DIR="$HOME/.nvm"
    [ -s "$HOMEBREW_PREFIX/opt/nvm/nvm.sh" ] && \. "$HOMEBREW_PREFIX/opt/nvm/nvm.sh" # This loads nvm
    [ -s "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm" ] && \. "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm" # This loads nvm bash_completion' >> ~/.bash_profile
```

### [Install pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation)

1. Consider installing with Homebrew:

```
brew install pyenv
```

2. Then follow the rest of the post-installation steps, starting with [Set up your shell environment for Pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#set-up-your-shell-environment-for-pyenv).
