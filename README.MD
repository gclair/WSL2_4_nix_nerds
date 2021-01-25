##### Table of Contents
- [WSL2_4_Nix_Nerds - Introduction](#wsl2_4_nix_nerds---introduction)
- [Pre-requisites](#pre-requisites)
- [Install WSL2](#install-wsl2)
- [Post-Install (the stuff you're here for)](#post-install-the-stuff-youre-here-for)
  - [Windows Terminal (WT) Settings](#windows-terminal-wt-settings)
    - [My WT Settings](#my-wt-settings)
- [Linux Configuration](#linux-configuration)
- [ZSH](#zsh)
- [VIM](#vim)
- [Tmux](#tmux)
  - [Terminal Setting](#terminal-setting)
  - [Cut and Paste](#cut-and-paste)
- [Controlling your instances](#controlling-your-instances)
- [References](#references)

# WSL2_4_Nix_Nerds - Introduction
A quickstart guide to getting Debian in WSL2 to an acceptable working state. If you have VMWare Workstation, Docker For Windows, or VirtualBox then the "VirtualMachinePlatform" feature ***MAY*** affect them by not allowing you to start up the VMs you have already built as this is run via Hyper-V. See [FAQ](https://docs.microsoft.com/en-us/windows/wsl/wsl2-faq#will-i-be-able-to-run-wsl-2-and-other-3rd-party-virtualization-tools-such-as-vmware-or-virtualbox)

# Pre-requisites
  * Windows 10 (WSL 2 is only available in Windows 10, Version 1903, Build 18362, WSL 1 for earlier versions)
  * Chocolately - Windows [package manager](https://chocolatey.org/install) which runs in Powershell (as Administrator). Its not necessary but will make finding and installing packages easier. 
  * Windows Terminal : https://docs.microsoft.com/en-us/windows/terminal/get-started
    * Possibly a better powerline/Unicode/Colour support (I have not tested this yet): WSLTTY https://github.com/mintty/wsltty 
  * TrueType Fonts (TTF): 
    * ***Recommended*** Cascadia Code NerdFont (Makes text look awesome) - `choco install cascadia-code-nerd-font` 
    * Microsoft's Cascadia Code Powerline (https://github.com/microsoft/cascadia-code/releases)

# Install WSL2
  Really just follow the steps (https://docs.microsoft.com/en-us/windows/wsl/install-win10), but here's the quick steps I used:
  In PowerShell as Administrator
  * `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
  * `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
  * `Restart-Computer`
  * Install updated Linux Kernel : https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
  * Set default WSL to version 2 :  `wsl --set-default-version 2`
  * Install your preferred distro from the [MS Store](https://aka.ms/wslstore)
  * Run your installation and go through the install and initialization steps, username and password
  * (Optional) VSCode On Windows (PowerShell as Administrator) - `choco install vscode`
  
# Post-Install (the stuff you're here for)
 ## Windows Terminal (WT) Settings
  * WT Settings - Can be reached via Windows Terminal Settings, `Ctrl+,` or click on the down chevron next to the `+` symbol in the tabline. If you haven't configured an editor for `.json` files, it will ask you for one. I'm using VSCode.
  * See the [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/customize-settings/global-settings) page for detailed customization options. This is where behaviour and look and feel (background images, transparency etc) are defined.
  * Useful trick: If you have Windows Terminal running and while you have the configuration file open, when you save settings, Terminal will reload them right away and you can see what effect you've had on it right away.
  
To make your default terminal instance your unix instance, set the `"defaultProfile"` attribute to the GUID value of the Unix install from further down in the `"profiles"` list. Altering the order of this list will alter the order of your shell choices when launching WT. 

  ###  My WT Settings 
  * Launch your Debian instance via right-click on the Windows Terminal icon and choosing your Debian instance
  * Choose your font, font size, and set your default start directory will be the `c:\users\<username>\` directory. If you perfer good old fashioned `/home/<username>` then set it as per below. There are different ways fonts could be installed, either via `choco` or manually downloaded from [Nerd Fonts](https://www.nerdfonts.com). Make sure you have the correct font name by using the "Font" tool in the Start menu. You can put these in the `"default"` section of the `"profiles"`, or per profile. These examples are per profile and in the `"name":"Debian"` profile for myself. Order of attributes doesn't seem to matter. 
```
      "startingDirectory": "//wsl$//Debian//home//gclair",
      //"fontFace": "MesloLGS NF",
      "fontFace": "CaskaydiaCove Nerd Font Mono",
      "fontSize": 10,
```
   * Copy on Selection: `"copyOnSelect": true` defaults to `false`. Selection is copied to clipboard on selection. Right click will then always paste. 
   * Paste Warnings (can be annoying): `"largePasteWarning"` or `"multiLinePasteWarning"` set to `true` or `false` per users choice. Defaults to `true`
    
# Linux Configuration
  * DNS is usually broken - Networking info is obtained via WSL Ethernet interface and gets its own private host only subnet. Edit `/etc/wsl.conf` with the following to "permanently" fix it:
```
[network]
generateResolvConf = false
```
  * Restart your Debian instance (Powershell): wsl -t Debian, launch WT Debian instance
  * Fix resolv.conf (should no longer be updating): `sudo rm /etc/resolv.conf && sudo echo nameserver 8.8.4.4 >/etc/resolv.conf` - Yes its Google's NS, but i have flaky issues with Cloudflare it seems. 
  * Debian doesn't install `man` by default or other useful packages:  `sudo apt update && sudo apt install dnsutils vim man curl wget zsh git build-essential` 
  * [Homebrew on Linux](https://brew.sh/) - I find distribution repos don't necessarily have all the up to date/best versions of software. Brew won't install until you fix your networking and get curl or wget installed. Add brew and its packages to your $PATH : `/home/linuxbrew/.linuxbrew/bin`
  * **FUN FACT** You can launch your Windows Executables from here. Try it! If you have VSCode in Windows just run `code`. It will launch the Windows version of code that recognizes WSL! See the YouTube link in the references. For fun, hack yourself! `calc.exe` ..  Yes its a joke. 

# ZSH
* Switch to zsh: `chsh -s /bin/zsh <yourusername>` , restart WT session.
  
 I like the usage of plugins with ZSH using [Oh My Zsh!](https://ohmyz.sh/) and [Powerlevel10k](https://github.com/romkatv/powerlevel10k) which really exercise those NerdFonts well. They recommend using the [Meslo Nerd Font](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k).   

# VIM
 I am a user of [SpaceVim](https://spacevim.org). It looks pretty and works relatively well for me. I didn't have to do many changes here. If you're an experienced dev, you have all your customizations in your github ready to go. I however, no not `¯\_(ツ)_/¯`

# Tmux
  This needs a section all its own, but if you got this far you know what you're doing.
  ## Terminal Setting
   I think the important part of tmux configuration is setting the terminal in your `~/.tmux.conf.local` OR `~/.tmux.conf` :   
    `set -g default-terminal "xterm-256color"` or to `"screen-256color"`. The xterm-256color seems to work for me.

   ## Cut and Paste
   ***WIP***  Tmux settings, copy to OS clipboard via ` | clip.exe`


# Controlling your instances
  * Instances are managed via powershell (admin) with `wsl`.
  * `wsl --list --verbose` OR `wsl -l -v` : list your installed instances
  * `wsl -s Debian` : Set your default distribution to start to the named "Debian" distro.
  * `wsl -t Debian` : Terminate the instance. Sometimes this is the 'reset' button you need 
  * I've never had to start the WSL2 instance as this is done automatically when you start Windows Terminal *nix instance.
  
# References
  * https://docs.microsoft.com/en-us/windows/wsl/
  * https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup
  * https://www.aaron-powell.com/posts/2020-04-24-a-walk-through-of-my-terminal-setup/
  * https://www.aaron-powell.com/posts/2020-03-25-how-i-setup-a-windows-dev-environment/
  * https://youtu.be/j0PPcUUtHlw <- Explanation of WSL1 vs WSL2, tips and tricks and making things pretty and functional.
