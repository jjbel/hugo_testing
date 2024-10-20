# Hugo Testing

## Prerequisites

### vscode

Code editor

https://code.visualstudio.com/download#

### Powershell

command-line shell

https://github.com/PowerShell/PowerShell/releases/download/v7.4.5/PowerShell-7.4.5-win-x64.msi

### Git

Version control

https://github.com/git-for-windows/git/releases/download/v2.47.0.windows.1/Git-2.47.0-64-bit.exe

### NodeJS

Javascript runtime for building sites

https://nodejs.org/dist/v20.18.0/node-v20.18.0-x64.msi

<!-- ### Chocolatey

package manager for windows

```ps
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
``` -->

### Hugo

static site generator

```ps
winget install Hugo.Hugo.Extended
```

## Create the site with Hugo

```ps
hugo new site . --force
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
```
