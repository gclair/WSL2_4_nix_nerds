# WSL2_4_nix_nerds
A quickstart guide to getting Debian in WSL2 to an acceptable working state. If you have VMWare Workstation, Docker For Windows, or VirtualBox the "VirtualMachinePlatform" feature may affect them by not allowing you to start up the VMs. 

## Pre-requisites
  * Windows 10 (WSL 2 is only available in Windows 10, Version 1903, Build 18362, WSL 1 for earlier versions )
  * Windows Terminal : https://docs.microsoft.com/en-us/windows/terminal/get-started
    * Alternatively better powerline/Unicode/Colour support: WSLTTY https://github.com/mintty/wsltty 
  * TrueType Fonts (TTF): 
    * Apple's SF Mono Powerline (https://github.com/Twixes/SF-Mono-Powerline)
    * Microsoft's Cascadia Code Powerline (https://github.com/microsoft/cascadia-code/releases)
 
## Install WSL2
  Really just follow the steps (https://docs.microsoft.com/en-us/windows/wsl/install-win10), but here's the quick and easy ones I used:
  In PowerShell as Administrator
  * `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
  * `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
  * `Restart-Computer`
  * Download && install : https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
  * Set default WSL to version 2 : `wsl --set-default-version 2`
  * Install your preferred distro from the [MS Store](https://aka.ms/wslstore)

## Post-Install (the stuff you're here for)
  
## References
  * https://docs.microsoft.com/en-us/windows/wsl/
  * https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup
  * https://www.aaron-powell.com/posts/2020-04-24-a-walk-through-of-my-terminal-setup/
  * https://www.aaron-powell.com/posts/2020-03-25-how-i-setup-a-windows-dev-environment/
  
