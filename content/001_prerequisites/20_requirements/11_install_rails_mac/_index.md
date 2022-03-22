---
title: "Mac Setup"
date: 2022-03-22T12:29:19+02:00
draft: false
weight: 10
---

### Overview
{{% notice info%}}
🕘 This will take about 30 minutes.
{{% / notice %}}

We will be setting up a Ruby on Rails development environment on macOS 11 Big Sur.

Older versions of OS are mostly compatible so follow along as far as you can and then Google search for any problems you run into. There are plenty of people who have documented solutions for them.

#### Using ZSH in your Terminal
MacOS Catalina has changed the default terminal from Bash to ZSH. As a result, we'll be adding configs to ~/.zshrc instead of ~/.bash_profile like we used in the past.

You can manually change from Bash to ZSH anytime by running the following command:

```terminal
chsh -s /bin/zsh
```

#### Installing Homebrew

First, we need to install [Homebrew](https://brew.sh/). Homebrew allows us to install and compile software packages easily from source.

Homebrew comes with a very simple install script. When it asks you to install XCode CommandLine Tools, say yes.

Open Terminal and run the following command:

```terminal
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Installing Ruby
{{% notice info %}}
Recommended version: Ruby 3.1.1
{{% /notice %}}

Now that we have Homebrew installed, we can use it to install Ruby.

We're going to use [rbenv](https://github.com/sstephenson/rbenv) to install and manage our Ruby versions.

To do this, run the following commands in your Terminal:

```terminal
brew install rbenv ruby-build

# Add rbenv to bash so that it loads every time you open a terminal
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc
source ~/.zshrc
```

To install Ruby and set the default version, we'll run the following commands:

```text
rbenv install 3.1.1
rbenv global 3.1.1
```

Confirm the default Ruby version matches the version you just installed.

```text
ruby -v
```

#### Configuring Git

We'll be using Git for our version control system so we're going to set it up to match our Github account. If you don't already have a Github account, make sure to [register](https://github.com/). It will come in handy for the future.

Replace my name and email address in the following steps with the ones you used for your Github account.

```terminal
git config --global color.ui true
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR@EMAIL.com"
ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"
```

The next step is to take the newly generated SSH key and add it to your Github account. You want to copy and paste the output of the following command and [paste it here](https://github.com/settings/ssh).

```terminal
cat ~/.ssh/id_rsa.pub
```

Once you've done this, you can check and see if it worked:
```terminal
Hi felender7! "You've successfully authenticated, but GitHub does not provide shell access."
```

#### Installing Rails 
{{% notice info %}}
Recommended version: Rails 7.0.2.3
{{% / notice  %}}

Installing Rails is as simple as running the following command in your Terminal:

```terminal
gem install rails -v 7.0.2.3
```

Rails is now installed, but in order for us to use the `rails` executable, we need to tell `rbenv` to see it:

```terminal
rbenv rehash
```

And now we can verify Rails is installed:

```terminal
rails -v

# Rails 7.0.2.3
```

#### Setting Up A Database

We're going to install sqlite3 from homebrew because we can't use the built-in version with macOS Sierra without running into some troubles.

```terminal
brew install sqlite3
```

Rails ships with sqlite3 as the default database. Chances are you won't want to use it because it's stored as a simple file on disk. You'll probably want something more robust like MySQL or PostgreSQL.

There is a lot of documentation on both, so you can just pick one that seems like you'll be more comfortable with.

**If you're new to Ruby on Rails or databases in general, I strongly recommend [setting up PostgreSQL](https://gorails.com/setup/osx#postgresql).**

If you're coming from PHP, you may already be familiar with MySQL.

### MySQL

You can install [MySQL](https://www.mysql.com/) server and client from Homebrew:

```terminal
brew install mysql
```

Once this command is finished, it gives you a couple commands to run. Follow the instructions and run them:

```terminal
# To have launchd start mysql at login:
brew services start mysql
```

By default the mysql user is `root` with no password.

When you're finished, you can skip to the Final Steps.

#### PostgreSQL
You can install [PostgreSQL](https://www.postgresql.org/) server and client from Homebrew:

```terminal
brew install postgresql
```

Once this command is finished, it gives you a couple commands to run. Follow the instructions and run them:

```terminal
# To have launchd start postgresql at login:
brew services start postgresql
```

By default the postgresql user is your current OS X username with no password. For example, my OS X user is named `felender` so I can login to postgresql with that username.

### Final Steps
Mojave changed the location of header files necessary for compiling C extensions. You might need to run the following command to install pg, nokogiri, or other gems that require C extensions:

```terminal
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```
And now for the moment of truth. Let's create your first Rails application:

```terminal
rails new myapp

#### If you want to use MySQL
rails new myapp -d mysql

#### If you want to use Postgres
# Note you will need to change config/database.yml's username to be
# the same as your OSX user account. (for example, mine is 'felender')
rails new myapp -d postgresql

# Move into the application directory
cd myapp

# If you setup MySQL or Postgres with a username/password, modify the
# config/database.yml file to contain the username/password that you specified

# Create the database
rake db:create

rails server
```

You can now visit [http://localhost:3000](http://localhost:3000) to view your new website!

Now that you've got your machine setup, it's time to start building some Rails applications.

If you received an error that said Access denied for user 'root'@'localhost' (using password: NO) then you need to update your config/database.yml file to match the database username and password.

Thats it! `Happy Coding`