+++
title = "What I should install on my Mac?"
date = "2020-01-03"
draft = false
Categories = ["devops", "mac", "english"]
Tags = ["english", "devops", "mac", "brew", "saas"]
+++

Starting the year, I decided to do a backup and format my Macbook, restart from scratch. I am an ops guy. My setup is based on tools to create and manage automated infrastructure, SaaS services, and containers.

I started a twitter [thread](https://twitter.com/gomex/status/1212585682650177537) (Portuguese only) to get some good options.

I created this article to document the setup to myself and share it with the community to receive more feedback too. Please share your opinions.

- [.1. Using brew](#1-using-brew)
- [.2. Applications outside brew](#2-applications-outside-brew)
  * [.2.1. Dash](#21-dash)
  * [.2.2. Amphetamine](#2-2-amphetamine)
  * [.2.3. Oh My ZSH](#2-3-oh-my-zsh)
- [.3. Generic apps (Using Brew)](#3-generic-apps--using-brew-)
  * [.3.1. Firefox e Chrome](#3-1-firefox-e-chrome)
  * [.3.2. Iterm2](#3-2-iterm2)
  * [.3.3. Slack](#3-3-slack)
  * [.3.4. Spectacle](#3-4-spectacle)
  * [.3.5. Telegram](#3-5-telegram)
  * [.3.6. Whatsapp](#3-6-whatsapp)
  * [.3.7. Transmission](#3-7-transmission)
  * [.3.8. Spotify](#3-8-spotify)
  * [.3.9. Flycut](#3-9-flycut)
- [.4. Automation apps (With Brew)](#4-automation-apps--with-brew-)
  * [.4.1. Visual Studio Code](#4-1-visual-studio-code)
  * [.4.2. JQ](#4-2-jq)
  * [.4.3. Nmap](#4-3-nmap)
  * [.4.4. Watch](#4-4-watch)
  * [.4.5. Docker](#4-5-docker)
  * [.4.6. Docker Compose](#4-6-docker-compose)
  * [.4.7. Kubernetes-cli](#4-7-kubernetes-cli)
  * [.4.8. Popeye](#4-8-popeye)
  * [.4.9. Stern](#4-9-stern)
  * [.4.10. Kubectx + Kubens](#4-10-kubectx---kubens)
  * [.4.11. Insomnia](#4-11-insomnia)
- [.5. Thanks](#5-thanks)

## .1. Using brew

Everybody that uses Mac should use brew to install your packages, if you don't use it, please consider this as my first recommendation of this article.

The brew is a tool to install packages on your Macbook using a CLI command ```brew install package_name```

Brew has an outstanding [feature](https://github.com/Homebrew/homebrew-bundle) to install all packages that you need using just one command and one file with the complete list of apps.

To use it you need to create a ```Brewfile``` with the list following this convention:

tap "homebrew/bundle"
tap "homebrew/cask"
tap "homebrew/core"
cask "alfred"
brew "jq"

To understand the ```Brewfile```: **tap** is the repository to get the app, **cask** is to install this app using cask and **brew** using brew.  Please read this document (https://apple.stackexchange.com/questions/125468/what-is-the-difference-between-brew-and-brew-cask) to understand the difference between brew and brew cask.

When you have this file, you need to run this command:

```
brew bundle install
```
Here is my [Brewfile](https://github.com/gomex/mac-setup/blob/master/Brewfile). 


## .2. Applications outside brew

You can't find some apps on the brew, and because of it, I will explain first the apps that you should install using other methods.

### .2.1. Dash

 - Website: [https://kapeli.com/dash](https://kapeli.com/dash)
 - Price: Free with limitations

Dash is an API Documentation Browser and Code Snippet Manager. The best thing is that you can search offline too.

### .2.2. Amphetamine

 - Website: [https://apps.apple.com/us/app/amphetamine/id937984704?mt=12](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12)
 - Price: Free

The simple app used to keep your Mac awake on a specific time frame.  This is really useful on presentations, can you imagine your Mac hibernate in the middle of your talk because you didn't use your power supply? Amphetamine can save you.

### .2.3. Oh My ZSH

 - Website: [https://ohmyz.sh/](https://ohmyz.sh/)
 - Price: Opensource

Do you use a zsh shell? If yes, you should consider this community-driven framework for managing your zsh configuration.

## .3. Generic apps (Using Brew)

I will separate the details of apps (using brew) in two categories:

Generic: Apps that everyone could install. These are not related to my role.
Automation: Apps related to my role (Infrastructure Automation).

### .3.1. Firefox e Chrome

 - Website: [https://www.google.com/intl/pt-BR/chrome/](https://www.google.com/intl/pt-BR/chrome/) e [https://www.mozilla.org/pt-BR/firefox/new/](https://www.mozilla.org/pt-BR/firefox/new/)
 - Price: Opensource (Firefox) and Free (Chrome)

I don't need to explain these. IMHO it would be best if you had both because you need to troubleshoot problems of web apps in a particular browser.

### .3.2. Iterm2

 - Website: [https://iterm2.com/](https://iterm2.com/)
 - Price: Opensource

This terminal is impressive. So many features and it is more beautiful than the regular Mac terminal too.

### .3.3. Slack

 - Website: [https://slack.com/](https://slack.com)
 - Price: Free

This is used by all companies that I already worked, and we can't avoid to install it. This app has relevant problems to consume CPU and RAM memory, but we need to have it.

### .3.4. Spectacle

 - Website: [https://www.spectacleapp.com/](https://www.spectacleapp.com/)
 - Price: Free

You can find useful shortcuts to manage your app windows. Because of it, you need to install another tool to give you options. The Spectacle app is a good option. This app is no longer being actively maintained, but they are effortless. I am still using it.

My best shortcut is ```control + CMD + F``` to force my apps's windows to use all the space available on the screen and don't enter in "full screen mode".

If you wanna try another option, you may install [Rectangle](https://github.com/rxhanson/Rectangle).

### .3.5. Telegram

 - Website: https://telegram.org/
 - Price: Opensource

IMHO the best chat app ever, simple, clean, and enjoyable features (poor security, I know). This tool is really used for the Brazillian IT community (i.e., Dockerbr group has ~5k members).

### .3.6. Whatsapp

 - Website: [https://www.whatsapp.com/](https://www.whatsapp.com/)
 - Price: Free

This app is viral in Brazil, most people use this, and we can't avoid it to talk with families and friends outside the "tech world".

### .3.7. Transmission

 - Website: [https://transmissionbt.com/](https://transmissionbt.com/)
 - Price: Opensource

IMHO the most straightforward BitTorrent client for Mac. 

### .3.8. Spotify

 - Website: [https://www.spotify.com/br/](https://www.spotify.com/br/)
 - Price: Free with limitations

"Spotify is a digital music service that gives you access to millions of songs." I love songs, if you like it too, you may install and pay for it.

### .3.9. Flycut

 - Website: [https://apps.apple.com/br/app/flycut-clipboard-manager/id442160987?mt=12](https://apps.apple.com/br/app/flycut-clipboard-manager/id442160987?mt=12)
 - Price: Free

"Flycut is a clean and simple clipboard manager for developers. Every time you copy code pieces Flycut store it in history. Later you can past it using Shift-Command-V even if you have something different in your current clipboard. You can change hotkey and other settings in preferences."

## .4. Automation apps (With Brew)

### .4.1. Visual Studio Code

 - Website: [https://code.visualstudio.com/](https://code.visualstudio.com/)
 - Price: Opensource

"Visual Studio Code is a code editor redefined and optimized for building and debugging modern web and cloud applications." Microsoft won this "race". IMHO VScode is the better code editor. 

### .4.2. JQ

 - Website: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)
 - Price: Opensource

If you need to handle data on JSON format, this tool may help you. JQ is considered the ```sed``` of JSON documents.  You can learn more about this tool [here](https://stedolan.github.io/jq/tutorial/). 

### .4.3. Nmap

 - Website: [https://nmap.org/](https://nmap.org/)
 - Price: Opensource

If you need to check open ports and other network discoveries, you should consider to install it. You can install plugins to extend the tool, and they have a vast community with a lot of documents and new ideas of usage too.

### .4.4. Watch

 - Website: [https://en.wikipedia.org/wiki/Watch_(Unix)](https://en.wikipedia.org/wiki/Watch_(Unix))
 - Price: Opensource

If you need to run the same command continuous to refresh the data (.i.e., ps to check process), you should use the watch to don't add too many entries on your shell history and keep it updating until you are free to check something else.

### .4.5. Docker

 - Website: [https://docs.docker.com/docker-for-mac/](https://docs.docker.com/docker-for-mac/)
 - Price: Opensource 

If you use containers these days, probably you should install this app. This app is Docker for Mac and provides you a virtual machine to build/use your Linux docker images on your Mac.

I use docker to run other binaries and avoid the necessity to install it. For example:

```
docker run -it -v $PWD:/app -w /app --entrypoint="" terraform:light sh
```

I can use the terraform, and I didn't install it on my Mac, and I can specify the version of terraform too.

### .4.6. Docker Compose

- Website: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
- Price: Opensource

This tool is handy to set up a sophisticated container set up locally.  If you work with containers, you should install it.

### .4.7. Kubernetes-cli

 - Website: [https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos)
 - Price: Opensource

If you work with Kubernetes, you need this tool. This is the CLI binary to interact with your Kubernetes cluster.

### .4.8. Popeye

 - Website: [https://github.com/derailed/popeye](https://github.com/derailed/popeye)
 - Price: Opensource

If you use Kubernetes, this tool can help you a lot. You can use it to scan your Kubernetes cluster, and popeye will give you a report with possible problems on your resources and configuration.

### .4.9. Stern

 - Website: [https://github.com/wercker/stern](https://github.com/wercker/stern)
 - Price: Opensource

Do you need to troubleshoot Kubernetes pods? This tool can help you to  "tail" multiple pod's logs of a Kubernetes cluster.

### .4.10. Kubectx + Kubens

 - Website: [https://github.com/ahmetb/kubectx](https://github.com/ahmetb/kubectx)
 - Price: Opensource

Do you use multiple clusters/namespaces Kubernetes? This tool can help you to switch cluster and namespace smoothly.

### .4.11. Insomnia

 - Website: [https://insomnia.rest/](https://insomnia.rest/)
 - Price: Opensource

Do you need to test REST or GraphQL API? This straightforward tool can help with that.  You may use postman, this is an alternate option.

## .5. Thanks

 - Everyone here: [thread](https://twitter.com/gomex/status/1212585682650177537).
 - [@somatorio](https://twitter.com/Somatorio)
 - [@jjunior0x2A](https://twitter.com/jjunior0x2A)
 - [@badtux_](https://twitter.com/badtux_)
 - [@@jabezerra](https://twitter.com/jabezerra)
 - [@malaquiasdev](https://twitter.com/malaquiasdev)
 