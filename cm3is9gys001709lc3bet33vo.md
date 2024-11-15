---
title: "Installing Freqtrade/FreqAI in new Linux Distro"
seoTitle: "Freqtrade & FreqAI Setup on Linux Distro"
seoDescription: "Learn how to install Freqtrade in a new Linux distro using WSL, including overcoming dependencies and setting up Python and TA-Lib"
datePublished: Fri Nov 15 2024 13:36:59 GMT+0000 (Coordinated Universal Time)
cuid: cm3is9gys001709lc3bet33vo
slug: installing-freqtradefreqai-in-new-linux-distro
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731677999171/7e77ca29-7153-4ae3-bc31-628b651139e1.webp
tags: linux, wsl, freqtrade

---

# Using WSL

As my main machine to run bots locally and run the model training is a laptop running windows while I usually use Linux on VPS service, I chose to opt-out for Windows subsystem for Linux(WSL) to have less friction I am ready to run bots on VPS.

This guide is not a detailed guide but only main topic guideline of steps how to run Freqtrade on WSL.

## Installing Linux Distro & Connect to WSL

I will be mainly using WSL extension in Vscode to install and run Linux Distro.

After install the extension run (Ctrl + Shift + P)  
`> WSL: Connect to WSL`

The extension will prompt and guide you to install the distro if no Distro has already been install. Otherwise Vscode will connect to the WSL.

# Requirements

## Python, Pip, vEnv and Git

These are usually for fresh environment e.g. fresh installation of Linux distro.

```plaintext
# update repository
sudo apt-get update

# install packages
sudo apt install -y python3-pip python3-venv python3-dev python3-pandas git curl
```

## \*TA-Lib

Installing Ta-Lib is the most frustrating process as I remembered. Freqtrade actually have a script installer (.sh) to install but incase, and in my case, it doesn’t work. I chose to install manually.

Here is the official guide for installation: [How to install Ta-Lib](https://ta-lib.github.io/ta-lib-python/install.html)

To download ta-lib run:  
`wget` [`http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz`](http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz)

and run the following code.

```plaintext
$ tar zxvf ta-lib-0.4.0-src.tar.gz
$ cd ta-lib
$ ./configure --prefix=/usr
$ make
$ sudo make install
```

# Install FreqTrade

There is multiple ways to install Freqtrade, script installation, manual installation, with docker and with Conda. I prefer to use script installation and follow this [guide.](https://www.freqtrade.io/en/stable/installation/#script-installation)

In my experience installing and running bot in Conda usually have dependency issues with many packages as Conda has its own forge of library meaning the script won’t be able to install many package right away and have to be download manually later.

## Clone Freqtrade repo

```plaintext
git clone https://github.com/freqtrade/freqtrade.git
```

## Run installation command

In freqtrade folder run:

```plaintext
# --install, Install freqtrade from scratch
./setup.sh -i
```

### Installation success Message

```plaintext
----------------------------
Please use 'freqtrade new-config -c user_data/config.json' to generate a new configuration file.
----------------------------
----------------------------
Run the bot !
----------------------------
You can now use the bot by executing 'source .venv/bin/activate; freqtrade <subcommand>'.
You can see the list of available bot sub-commands by executing 'source .venv/bin/activate; freqtrade --help'.
You verify that freqtrade is installed successfully by running 'source .venv/bin/activate; freqtrade --version'.
```