---
title: "The Blog Application"
date: 2022-03-22T16:47:11+02:00
draft: false
weight: 1
---

###  Creating the Blog Application
Rails comes with a number of scripts called generators that are designed to make your development life easier by creating everything that's necessary to start working on a particular task. One of these is the new application generator, which will provide you with the foundation of a fresh Rails application so that you don't have to write it yourself.

To use this generator, open a terminal, navigate to a directory where you have rights to create files, and run:

```terminal
$ rails new blog
```

This will create a Rails application called Blog in a blog directory and install the gem dependencies that are already mentioned in Gemfile using bundle install.

{{%notice info%}}
You can see all of the command line options that the Rails application generator accepts by running rails new --help.
{{% / notice %}}

After you create the blog application, switch to its folder:

```terminal
$ cd blog
```
{{%notice tip%}}
Press `tab key` to auto complete
{{% / notice%}}

The `blog` directory will have a number of generated files and folders that make up the structure of a Rails application. Most of the work in this tutorial will happen in the app folder, but here's a basic rundown on the function of each of the files and folders that Rails creates by default:

<table>
<thead>
<tr>
<th>File/Folder</th>
<th>Purpose</th>
</tr>
</thead>
<tbody>
<tr>
<td>app/</td>
<td>Contains the controllers, models, views, helpers, mailers, channels, jobs, and assets for your application. You'll focus on this folder for the remainder of this guide.</td>
</tr>
<tr>
<td>bin/</td>
<td>Contains the <code>rails</code> script that starts your app and can contain other scripts you use to set up, update, deploy, or run your application.</td>
</tr>
<tr>
<td>config/</td>
<td>Contains configuration for your application's routes, database, and more. This is covered in more detail in <a href="configuring.html">Configuring Rails Applications</a>.</td>
</tr>
<tr>
<td>config.ru</td>
<td>Rack configuration for Rack-based servers used to start the application. For more information about Rack, see the <a href="https://rack.github.io/">Rack website</a>.</td>
</tr>
<tr>
<td>db/</td>
<td>Contains your current database schema, as well as the database migrations.</td>
</tr>
<tr>
<td>Gemfile<br>Gemfile.lock</td>
<td>These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the <a href="https://bundler.io">Bundler website</a>.</td>
</tr>
<tr>
<td>lib/</td>
<td>Extended modules for your application.</td>
</tr>
<tr>
<td>log/</td>
<td>Application log files.</td>
</tr>
<tr>
<td>public/</td>
<td>Contains static files and compiled assets. When your app is running, this directory will be exposed as-is.</td>
</tr>
<tr>
<td>Rakefile</td>
<td>This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing <code>Rakefile</code>, you should add your own tasks by adding files to the <code>lib/tasks</code> directory of your application.</td>
</tr>
<tr>
<td>README.md</td>
<td>This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.</td>
</tr>
<tr>
<td>storage/</td>
<td>Active Storage files for Disk Service. This is covered in <a href="active_storage_overview.html">Active Storage Overview</a>.</td>
</tr>
<tr>
<td>test/</td>
<td>Unit tests, fixtures, and other test apparatus. These are covered in <a href="testing.html">Testing Rails Applications</a>.</td>
</tr>
<tr>
<td>tmp/</td>
<td>Temporary files (like cache and pid files).</td>
</tr>
<tr>
<td>vendor/</td>
<td>A place for all third-party code. In a typical Rails application this includes vendored gems.</td>
</tr>
<tr>
<td>.gitignore</td>
<td>This file tells git which files (or patterns) it should ignore. See <a href="https://help.github.com/articles/ignoring-files">GitHub - Ignoring files</a> for more info about ignoring files.</td>
</tr>
<tr>
<td>.ruby-version</td>
<td>This file contains the default Ruby version.</td>
</tr>
</tbody>
</table>

### Starting up the Web Server
You actually have a functional Rails application already. To see it, you need to start a web server on your development machine. You can do this by running the following command in the blog directory:
```terminal
$ bin/rails server
```

{{%notice info %}}

If you are using Windows, you have to pass the scripts under the `bin` folder directly to the Ruby interpreter e.g. `ruby bin\rails server`.
{{% / notice %}}

{{%notice info %}}
JavaScript asset compression requires you have a JavaScript runtime available on your system, in the absence of a runtime you will see an `execjs error` during asset compression. Usually macOS and Windows come with a JavaScript runtime installed. therubyrhino is the recommended runtime for JRuby users and is added by default to the Gemfile in apps generated under JRuby. You can investigate all the supported runtimes at ExecJS.
{{% / notice %}}

This will start up Puma, a web server distributed with Rails by default. To see your application in action, open a browser window and navigate to [http://localhost:3000](http://localhost:3000). You should see the Rails default information page:

![Rails Welcome page](https://guides.rubyonrails.org/images/getting_started/rails_welcome.png?classes=shadow&outline&width=100pc)


When you want to stop the web server, hit Ctrl+C in the terminal window where it's running. In the development environment, Rails does not generally require you to restart the server; changes you make in files will be automatically picked up by the server.

The Rails startup page is the smoke test for a new Rails application: it makes sure that you have your software configured correctly enough to serve a page.