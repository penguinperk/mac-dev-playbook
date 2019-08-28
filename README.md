# Mac Development Ansible Playbook

[![Build Status](https://travis-ci.org/geerlingguy/mac-dev-playbook.svg?branch=master)](https://travis-ci.org/geerlingguy/mac-dev-playbook)

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.

This is a work in progress, and is mostly a means for me to document my current Mac's setup. I'll be evolving this set of playbooks over time.

  - [osxc](https://github.com/osxc)

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. [Install Ansible](http://docs.ansible.com/intro_installation.html).
  3. Clone this repository to your local drive.
  4. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  5. Run `ansible-playbook main.yml -i inventory -K` inside this directory. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. 
    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

The tags available are: 
 - dotfiles
 - homebrew
 - mas        - Does not work anything after macOS 10.13
 - extra-packages
 - osx
 - vundle     - https://github.com/VundleVim/Vundle.vim
 - powerline  - https://powerline.readthedocs.io/en/master/
 - zsh        - https://github.com/robbyrussell/oh-my-zsh


## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - bash-completion
      - git
      - nmap
      - ssh-copy-id
      - openssl
      - wget
      - awscli
      - python2
      - python3
      - tmux
      - zsh
      - zsh-syntax-highlighting 
    
    mas_installed_apps:
      - { id: 926036361,  name: "LastPass Password Manager (4.3.0)" }
      - { id: 823766827,  name: "OneDrive (18.214.1021)" }
      - { id: 450545814,  name: "Solar App (1.00)" }
      - { id: 784801555,  name: "Microsoft OneNote (16.20)" }
      - { id: 1289197285,  name: "MindNode 5 (5.2.3)" }
      - { id: 497799835, name: "Xcode" }
    

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):
  - [visual-studio-code](https://code.visualstudio.com/)
  - [tunnelblick](https://tunnelblick.net/)
  - [teamviewer](https://www.teamviewer.com)
  - [launchy](https://www.launchy.net/)
  - [istat-menus](https://bjango.com/mac/istatmenus/)
  - [keepassx](https://www.keepassx.org/)
  - [Docker](https://www.docker.com/)
  - [Dropbox](https://www.dropbox.com/)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Homebrew](http://brew.sh/)
  - [LICEcap](http://www.cockos.com/licecap/)
  - [LimeChat](http://limechat.net/mac/)
  - [MacVim](http://macvim-dev.github.io/macvim/)
  - [nvALT](http://brettterpstra.com/projects/nvalt/)
  - [Sequel Pro](https://www.sequelpro.com/) (MySQL client)
  - [Skitch](https://evernote.com/skitch/)
  - [Slack](https://slack.com/)
  - [Vagrant](https://www.vagrantup.com/)

Packages (installed with Homebrew):
  - bash-completion
  - git
  - nmap
  - ssh-copy-id
  - openssl
  - wget
  - awscli
  - python2
  - python3
  - tmux
  - zsh
  - zsh-syntax-highlighting 

My [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions:

### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).

  1. Set JJG-Term as the default Terminal theme (it's installed, but not set as default automatically).
  2. Install [Sublime Package Manager](http://sublime.wbond.net/installation).
  3. Install all the apps that aren't yet in this setup (see below).
  4. Remap Caps Lock to Escape (requires macOS Sierra 10.12.1+).
  5. Set trackpad tracking rate.
  6. Set mouse tracking rate.
  7. Configure extra Mail and/or Calendar accounts (e.g. Google, Exchange, etc.).
  8. Resolve Mas setup issue: (https://github.com/mas-cli/mas/issues/164) 

### Applications/packages to be added:


### Configuration to be added:

  - I have vim configuration in the repo, but I still need to add the actual installation:
    ```
    mkdir -p ~/.vim/autoload
    mkdir -p ~/.vim/bundle
    cd ~/.vim/autoload
    curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
    cd ~/.vim/bundle
    git clone git://github.com/scrooloose/nerdtree.git
    ```

## Testing the Playbook

Many people have asked me if I often wipe my entire workstation and start from scratch just to test changes to the playbook. Nope! Instead, I posted instructions for how I build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which I can continually run and re-run this playbook to test changes and make sure things work correctly.

Additionally, this project is [continuously tested on Travis CI's macOS infrastructure](https://travis-ci.org/geerlingguy/mac-dev-playbook).

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

[Jeff Geerling](https://www.jeffgeerling.com/), 2014 (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).
