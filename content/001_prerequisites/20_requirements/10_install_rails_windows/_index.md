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
{{% notice info %}}
Recommended version: 3.1.1
{{% /notice %}}

The first step is to install some dependencies for Ruby.
``` powershell
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

To install Ruby and set the default version, we'll run the following commands:

```powershell
rbenv install 3.1.1
rbenv global 3.1.1
```

Confirm the default Ruby version matches the version you just installed.

```powershell
ruby -v
```

The last step is to install Bundler
```powershell
gem install bundler
```
rbenv users need to run rbenv rehash after installing bundler.

#### Configuring Git

We'll be using Git for our version control system so we're going to set it up to match our Github account. If you don't already have a Github account, make sure to [register](https://github.com/). It will come in handy for the future.

Replace my name and email address in the following steps with the ones you used for your Github account.

```powershell
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"
```

The next step is to take the newly generated SSH key and add it to your Github account. You want to copy and paste the output of the following command and [paste it here](https://github.com/settings/ssh).

```powershell
cat ~/.ssh/id_rsa.pub
```

Once you've done this, you can check and see if it worked:
```powershell
Hi felender7! "You've successfully authenticated, but GitHub does not provide shell access."
```

#### Installing Rails 
{{% notice info %}}
Recommended version: 7.0.2.3
{{% / notice  %}}

Since Rails ships with so many dependencies these days, we're going to need to install a Javascript runtime like NodeJS and a package manager called Yarn.

To install NodeJS and Yarn, we're going to add it using the official repository:

```powershell
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt update
sudo apt-get install -y nodejs yarn
```

And now, without further adieu:
```powershell
gem install rails -v 7.0.2.3
```

If you're using rbenv, you'll need to run the following command to make the rails executable available:

```powershell
rbenv rehash
```

Now that you've installed Rails, you can run the `rails -v` command to make sure you have everything installed correctly:
```powershell
rails -v

# Rails 7.0.2.3
```
In the case of a different result, your environment may not be setup properly.

#### Setting Up MySQL

Rails ships with sqlite3 as the default database. Chances are you won't want to use it because it's stored as a simple file on disk. You'll probably want something more robust like MySQL or PostgreSQL.

There is a lot of documentation on both, so you can just pick one that seems like you'll be more comfortable with.

If you're new to Ruby on Rails or databases in general, I strongly recommend [setting up PostgreSQL](https://gorails.com/setup/windows/10#postgresql).

If you're coming from PHP, you may already be familiar with MySQL.

You can install MySQL server and client from the packages in the Ubuntu repository. As part of the installation process, you'll set the password for the root user. This information will go into your Rails app's `database.yml` file in the future.

```powershell
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

Installing the `libmysqlclient-dev` gives you the necessary files to compile the `mysql2` gem which is what Rails will use to connect to MySQL when you setup your Rails app.

When you're finished, you can skip to the Final Steps.

#### Setting Up PostgreSQL

The easiest way to setup PostgreSQL is to install it on Windows using one of the Windows installers. You'll be able to run it in Windows and connect to it through Linux.

[Download PostgreSQL for Windows](https://www.postgresql.org/download/windows/) and install it.

Pay attention to the username and password you setup during installation of Postgres as you will use this to configure your Rails applications later to login to Postgres when your Rails app runs.

You'll also need to install the development headers on Ubuntu so you can install the pg gem.

```powershell
sudo apt install libpq-dev
```

#### Final Steps

The best place to develop Rails apps on the Windows Subsystem for Linux is to navigate to `/mnt/c`. This is actually the C: drive on Windows and it lets you use Sublime, Atom, VS Code, etc on Windows to edit your Rails application.

{{% button href="https://code.visualstudio.com/download" icon="fas fa-download" %}}Download Visual Studio Code{{% /button %}}

And now for the moment of truth. Let's create your first Rails application:

```powershell
# Navigate to the C: drive on Windows. Do this every time you open the Linux console.
cd /mnt/c

# Create a code directory at C:\code for your Rails apps to live (You only need to do this once)
mkdir -p code

#### If you want to use Postgres
# Note that this will expect a postgres user with the same username
# as your app, you may need to edit config/database.yml to match the
# user you created earlier
rails new myapp -d postgresql

#### or if you want to use SQLite (not recommended)
# rails new myapp

#### Or if you want to use MySQL
# rails new myapp -d mysql

# Then, move into the application directory
cd myapp

# If you setup MySQL or Postgres with a username/password, modify the
# config/database.yml file to contain the username/password that you specified

# Create the database
rake db:create

rails server
```

You can now visit [http://localhost:3000](http://localhost:3000) to view your new website!

Now that you've got your machine setup, it's time to start building some Rails applications!

If you received an error that said `Access denied for user 'root'@'localhost' (using password: NO)` then you need to update your config/database.yml file to match the database username and password.

To edit your code in Windows: open up Sublime, Atom, VS Code, or whatever editor your prefer and point them to your folders in `C:\code` and you'll be able to safely edit your Rails apps in Windows and run them in Linux!

When you create a new Rails app, you might run into the following error: parent directory is world writable but not sticky.

If you run into this issue, you can run `chmod +t -R ~/.bundle` and that should fix the permissions errors and let you finish the bundle install for your Rails app.

That's it! `Happy Coding`