---
title: Setting Up a New macOS
date: 2024-01-06
description: "Guide for Setting up macOS Development Environment: Essential Software, Tools, and Browsers with Enhanced Privacy and Security Settings."
tags: ["cybersecurity", "developers", "privacy", "resources"]
type: "article"
---

# Setting up your new macOS

I predominantly use Apple products, and when transitioning to a new device within the Apple ecosystem, such as a fresh new MacBook, I find myself in the routine task of reinstalling all the essential software and tools crucial for my projects and day to day.

This article serves as a general to-do list for my own reference during the setup of a new laptop. Moreover, it can be a valuable source of inspiration for fellow individuals aiming to establish a privacy-conscious developer environment, encompassing the essential and often indispensable tools.

## macOS Privacy & Security Settings

[Change Privacy & Security Advanced settings on Mac](https://support.apple.com/en-gb/guide/mac-help/mh40595/mac).

Turn on [FileVault](https://support.apple.com/en-gb/guide/mac-help/mh11785/mac).

Configure [firmware password](https://support.apple.com/en-au/102384).

> Review the GitHub repository [drduh/macOS-Security-and-Privacy-Guide](https://github.com/drduh/macOS-Security-and-Privacy-Guide).

## Install Objective-See's Tools

Follow the instructions to install [LuLu](https://github.com/objective-see/LuLu) firewall and [BlockBlock](https://objective-see.org/products/blockblock.html) monitor.

## Xcode & Command Line Tools

```
xcode-select --install
```

## Homebrew (brew)

First start by installing Homebrew. Go to [brew.sh](https://brew.sh/).

Opt out of [analytics](https://docs.brew.sh/Analytics):
```
brew analytics off
```

### Install Visual Studio Code (VS Code)

```
brew install --cask visual-studio-code
```

> Alternatively, install it manually from the [website](https://code.visualstudio.com/docs/setup/mac).

### Install Wireshark

```
brew install --cask wireshark
```

```
brew install --cask wireshark-chmodbpf
```

### Install git

```
brew install git
```

### Install Python

```
brew install python3
```

> When developing with Python, use [virtual environments](https://github.com/DavidJKTofan/CyberSec-resources/blob/master/MacOS_Commands.md#virtual-environment).

### Install npm

```
brew install node
```

### Update Everything

Updating `brew`, `npm`, `nvm` and their packages:
```
brew update && brew upgrade && brew autoremove && brew cleanup && brew doctor && npm install -g npm@latest && npm update -g && nvm install --lts --latest-npm && nvm alias default $(nvm version --lts) && npm cache clean
```

## Private Browser

Install and use [Brave](https://brave.com/) or [Firefox](https://www.mozilla.org/en-US/firefox/new/) – whichever you feel safer with.

Review browser configurations.

Additionally, install and configure [uBlock Origin](https://github.com/gorhill/uBlock) Add-On on Firefox, or [Brave Shields](https://brave.com/shields/) on Brave.

> Use the [Startpage](https://www.startpage.com/) or [DuckDuckGo](https://duckduckgo.com/) or [Ecosia](https://www.ecosia.org/) Search Engines.

## Encrypted DNS

Follow the guide on [connect to 1.1.1.1 using DoH clients](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/dns-over-https-client/#cloudflared).

Alternatively, [configure DoH on your browser](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/encrypted-dns-browsers/).

## E-Mail, Calendar, Drive, VPN, Password Manager

Sign up, configure and use the [Proton](https://proton.me/) suite of products.

## More...

Read more on [A Journey into Digital Privacy & CyberSec](/articles/a-journey-into-digital-privacy/).

More information and examples on my [GitHub Repository](https://github.com/DavidJKTofan/CyberSec-resources/blob/master/MacOS_Commands.md).

---

## Disclaimer

Educational purposes only.

This blog post is independent and not affiliated with, endorsed by, or necessarily reflective of the opinions of any entities mentioned.
