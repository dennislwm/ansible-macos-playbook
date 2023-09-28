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
- [Copy and edit the configuration to your preference](#copy-and-edit-the-configuration-to-your-preference)
- [Execution](#execution)
  - [Run ansible-playbook on your config file](#run-ansible-playbook-on-your-config-file)
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

|            Category            |                      Activity                      | Mac User |
|:------------------------------:|:--------------------------------------------------:|:--------:|
| Installation and Configuration | Copy and edit the configuration to your preference |   R,A    |
|           Execution            |      Run ansible-playbook on your config file      |   R,A    |

---
# 4. Requirements
## 4.1. Mac

1. Install [Homebrew](https://brew.sh).
2. Install Python (`brew install python`)
3. Install Ansible (`pip3 install ansible`)

---
# 5. Installation and Configuration
# 5.1. Copy and edit the configuration to your preference

1. Copy `default.config.yml` to `config.yml` and edit the configuration to your likings.
  - **Don't skip this, otherwise your computer will be provisioned like mine :)**
2. Roles
  - Installs Homebrew packages and app casks (Role `homebrew`)
  - Installs App Store apps with [`mas-cli`](https://github.com/mas-cli/mas) (Role `mas`)
  - Modifies MacOS settings (Role `settings`)
  - Changes the user shell, if configured (Role `shell`)

---
# 6. Execution
## 6.1 Run ansible-playbook on your config file

1. Run `ansible-playbook main.yml`. Enter your account password when prompted.
   - If you have a configuration stored elsewhere (e.g. in a dotfiles folders), run `ansible-playbook main.yml --extra-vars=@/path/to/my/config.yml`

---
# 8. Reference
## 8.1. Acknowledgements

This playbook is heavily inspired by
[Jeff Geerling's mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook).

The macOS settings (a.k.a. `defaults write`s) are mostly taken from
[Mathias Bynens' defaults scripts](https://mths.be/macos) or from one of the
dotfiles repos from [http://dotfiles.github.io](http://dotfiles.github.io).
