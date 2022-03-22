---
title: "Windows Setups"
date: 2022-03-22T03:40:12+02:00
draft: false
weight: 11
---

### Overview

We will be setting up a Ruby on Rails development environment on Windows 10.

The reason we're going to be using Bash on Ubuntu on Windows because it allows you to run Linux on your Windows machine. Most Ruby on Rails tutorials and dependencies work best on Linux, so this allows you to get the best of both worlds. A Windows machine for your day to day work, and a Linux subsystem for Ruby on Rails development.

This only works on 64-bit installations of Windows. This is also in beta, so this won't be nearly as robust as running a Linux virtual machine, but it can definitely do the basics well enough.

#### Installing the Windows Subsystem for Linux

Windows 10 allows you to run various Linux operating systems inside of Windows similar to a virtual machine, but natively implemented. We'll use this to install Ruby and run our Rails apps.

Open Powershell as Administrator and run:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
[Next install Ubuntu from the Microsoft Store.](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6)

{{< figure src="https://d2i2nj5el4wq1j.cloudfront.net/assets/windows/wsl2_store-cbe40594c638e9ef0eff648a1f92f388ae2e26f65e53cef22e25b251d809556e.png" >}}

Now open Ubuntu in the Start menu or by running wsl in PowerShell or the command prompt. You'll be asked to setup a new user for Ubuntu. Remember this password as it's what you'll use later on when installing packages with sudo.

{{< figure src="https://d2i2nj5el4wq1j.cloudfront.net/assets/windows/wsl2_ubuntu-7127367a55ac43e2637c62d10d802c14a18d0e3b3f1df6b60ce0476a4c103a7a.png" >}}

Congrats! You've now got a Ubuntu terminal on Windows. You'll use this to run your Rails server and other processes for development.

#### Installing Ruby

Recommended version: 3.1.1

The first step is to install some dependencies for Ruby.
```powershell
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev
```
{{% notice tip %}}
In order for this to work, Git must be installed first
{{% /notice %}}

Next we're going to be installing Ruby with a version manager called Rbenv.

Installing with rbenv is a simple two step process. First you install rbenv, and then ruby-build:
```powershell
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

```