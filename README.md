# Ansible MacOS Playbook

This is the playbook I use after a clean install of MacOS to set everything up.

<!-- TOC -->

- [Ansible MacOS Playbook](#ansible-macos-playbook)
- [Introduction](#introduction)
    - [Audience](#audience)
- [System Overview](#system-overview)
    - [Benefits and Values](#benefits-and-values)
- [User Personas](#user-personas)
    - [RACI Matrix](#raci-matrix)
- [Requirements](#requirements)
    - [Mac](#mac)
- [Installation and Configuration](#installation-and-configuration)
- [Clone the repo and copy the configuration](#clone-the-repo-and-copy-the-configuration)
- [Copy and edit the configuration to your preference](#copy-and-edit-the-configuration-to-your-preference)
    - [Configure git profile](#configure-git-profile)
- [Execution](#execution)
    - [Run ansible-playbook on your config file](#run-ansible-playbook-on-your-config-file)
    - [Find and automate default user settings for Mac](#find-and-automate-default-user-settings-for-mac)
- [Post-Execution](#post-execution)
    - [Mac](#mac)
- [Reference](#reference)
    - [Acknowledgements](#acknowledgements)

<!-- /TOC -->

---
# 1. Introduction

This document describes the MacOS automation to install personal or work apps on a clean Mac that is provided for Mac Users.

## 1.2. Audience

The audience for this document includes:

* Mac User who will install and configure apps on their workstation.

---
# 2. System Overview
## 2.1. Benefits and Values

1. One benefit is that you can have replicate your personal or work environment to multiple Mac workstations quickly.

---
# 3. User Personas
## 3.1 RACI Matrix

|            Category            |                    Activity                     | Mac User |
|:------------------------------:|:-----------------------------------------------:|:--------:|
| Installation and Configuration |    Clone the repo and copy the configuration    |   R,A    |
| Installation and Configuration |  Configure the config file to your preference   |   R,A    |
| Installation and Configuration |              Configure git profile              |   R,A    |
|           Execution            |    Run ansible-playbook on your config file     |   R,A    |
|           Execution            | Find and automate default user settings for Mac |   R,A    |

---
# 4. Requirements
## 4.1. Mac

1. Rename your Mac workstation.
  - Navigate to System Settings > General > About.
  - For the **Name**, enter a name for your workstation.
2. Install [Homebrew](https://brew.sh).
3. Change the default shell to bash for the current user.

```sh
chsh -s /bin/bash
echo $SHELL
```

4. Create a default bash profile with `nano ~/.bash_profile`.

```sh
export PATH=/opt/homebrew/bin:/opt/homebrew/sbin:$PATH
export PS1='\[\033[36m\]\u@\[\033[35m\]\h\[\033[m\]:\[\033[33;1m\]\w\[\033[32m\]\[\033[m\]$ '
export PROMPT_COMMAND='if [ "$(id -u)" -ne 0 ]; then echo "$(date "+%Y-%m-%d.%H:%M:%S") $(pwd) $(history 1)" >> ~/.bash_logs/bash-history-$(date "+%Y-%m-%d").log; fi'
# source ~/src/startup.sh
# source /opt/homebrew/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/completion.bash.inc
# source /opt/homebrew/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.bash.inc
```

5. Install Python (`brew install python`)
6. Install Ansible (`pip3 install ansible`)

---
# 5. Installation and Configuration
# 5.1. Clone the repo and copy the configuration

1. Open a terminal and clone the repo to your workstation.

```sh
mkdir git
cd git
git clone https://github.com/dennislwm/ansible-macos-playbook --branch dbmacm3-001-initial
cd ansible-macos-playbook
```

2. Copy `default.config.yml` to `config.yml` and edit the configuration to your likings.
  - **Don't skip this, otherwise your computer will be provisioned like mine :)**

# 5.2. Copy and edit the configuration to your preference

1. Change both `computername` and `hostname` to match your workstation name.

2. Under `homebrew_taps`, there should be at least one item in the array, e.g. `- homebrew/core`.

3. Under `homebrew_apps`, there should be at least one item in the array, e.g. `- google-chrome`.

4. Under `homebrew_packages`, there should be at least one item in the array, e.g. `- lastpass-cli`.

5. Under `mas_apps`, there should be at least one item in the array, e.g. `- { name: Moom, id: 419330170 }`.

6. Roles
  - Installs Homebrew packages and app casks (Role `homebrew`)
  - Installs App Store apps with [`mas-cli`](https://github.com/mas-cli/mas) (Role `mas`)
  - Modifies MacOS settings (Role `settings`)
  - Changes the user shell, if configured (Role `shell`)

7. Taps, Apps, Packages and Mac App Store
- homebrew_apps:
  - alt-tab: Alt-tab between windows
  - google-chrome: Chrome browser
  - visual-studio-code: Code editor
  - zerotier-one: Zero trust VPN to private LAN
- homebrew_packages:
  - mas: Mac App Store cli to search for name and id of apps
  - pipenv: Python environment manager
- mas_apps:
  - Moom: Window custom layout manager
  - Wireless@SGx: Free public WiFi hotspots around Singapore

```yml
---

computername: dbmacm3
hostname: dbmacm3

finder_proxy_icon_hover_delay: 0

system_save_application_state_on_quit: no
system_expand_save_panel_by_default: yes
system_expand_print_panel_by_default: yes
system_show_file_extensions_warning: no
system_warn_before_emptying_the_trash: yes
system_use_f_keys_as_standard_function_keys: yes
system_index_network_storage_with_spotlight: no
trackpad_tap_to_click: yes

screensaver_ask_for_password: no
screensaver_ask_for_password_in_seconds: 5

shell_path: /usr/local/bin/zsh

homebrew_taps:
  - homebrew/core

homebrew_apps:
  - alt-tab
  - google-chrome
  - visual-studio-code

homebrew_packages:
  - mas
  - pipenv

mas_apps:
  - { name: Moom, id: 419330170 }
  - { name: Wireless@SGx, id: 1449928544 }
```

## 5.3. Configure git profile

1. Open a terminal and type `git config --global --edit`.

2. Copy and paste the following content:

```sh
[core]
	editor = code --wait
[user]
	email = dennislwm@your.email
	name = Dennis Lee
[init]
	defaultBranch = main
[color]
	status = auto
	branch = auto
	interactive = auto
	diff = auto
[pull]
	rebase = false
[commit]
	template = /Users/dennislwm/config/commit.txt
```

---
# 6. Execution
## 6.1 Run ansible-playbook on your config file

1. Run `ansible-playbook main.yml`. Enter your account password when prompted.
   - If you have a configuration stored elsewhere (e.g. in a dotfiles folders), run `ansible-playbook main.yml --extra-vars=@/path/to/my/config.yml`

## 6.2. Find and automate default user settings for Mac

1. Open a terminal and type the following command to save current defaults:

```sh
defaults domains | tr ', ' '\n' > before.txt
```

2. Open the UI and change a setting. Then type the following command to check diff:

```sh
defaults domains | tr ', ' '\n' > after.txt
diff before.txt after.txt
```

3. Using the changed DOMAIN (application) and KEY above, check type and set the VALUE:

```sh
defaults read-type DOMAIN KEY
# type is boolean
defaults write DOMAIN KEY -bool VALUE
```

4. Kill any affected DOMAIN (application) to apply the changes:

```sh
killall DOMAIN 2> /dev/null
```

---
# 7. Post-Execution
## 7.1. Mac

1. Turn on sync settings in Visual Studio Code using your GitHub account.
2. Git clone, install and configure [dotfiles](https://github.com/dennis/bash-alias-custom).

---
# 8. Reference
## 8.1. Acknowledgements

This playbook is heavily inspired by
[Jeff Geerling's mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook).

The macOS settings (a.k.a. `defaults write`s) are mostly taken from
[Mathias Bynens' defaults scripts](https://mths.be/macos) or from one of the
dotfiles repos from [http://dotfiles.github.io](http://dotfiles.github.io).
