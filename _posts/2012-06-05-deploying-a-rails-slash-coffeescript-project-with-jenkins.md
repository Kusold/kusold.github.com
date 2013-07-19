---
layout: post
title: "Deploying a Rails/Coffeescript Project with Jenkins"
date: 2012-06-05 11:11
comments: true
categories: Development
published: true
---

I was recently asked to setup [Jenkins](http://jenkins-ci.org/) on our CI Box that is running [CentOS 6](http://www.centos.org/). I wasn't able to find a good guide, so I decided to create my own.

### Installation ###

The first thing to do is to install development tools. Not all of these are needed, but they will save headaches in the future.

	$ yum groupinstall 'Development Tools'

Next, the default version of Java included in CentOS is not compatible with Jenkins. You will need to remove the current version, and install Java 1.6.

	$ yum remove java
	$ yum install java-1.6.0-openjdk

The [Jenkins wiki](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+RedHat+distributions) doesn't mention this, but I had trouble running Jenkins without Tomcat 6.

	$ yum install tomcat6
	$ wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
	$ rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
	$ yum install jenkins

Start up Jenkins as a service:
	$ sudo service jenkins start

Now Jenkins should be up and running, navigate to [http://localhost:8080](http://localhost:8080) in order to verify that it is working.

### Useful Plugins ###

* [GitHub Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin) - Integration between Jenkins and GitHub.
* [Rake Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Rake+plugin) - Allows Jenkins to invoke Rake tasks as build steps.
* [Ruby Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Ruby+Plugin) - This plugin will let users use Ruby in the build scripts.
* [RVM Plugin](https://wiki.jenkins-ci.org/display/JENKINS/RVM+Plugin) - Runs your jobs in the RVM managed ruby+gemset of your choice.
* [Green Balls](https://wiki.jenkins-ci.org/display/JENKINS/Green+Balls) - Changes Jenkins to use green balls instead of blue for successful builds.

### GitHub Integration ###

In order to get Jenkins to be able to pull a private repo from [GitHub](http://github.com), you will need to generate private and public keys. I found the easiest way to do this is as the `jenkins` user.

	$ su -l -p jenkins
	$ ssh-keygen -t rsa -C "your_email@youremail.com"

Now upload the public key to Github, and test to verify it worked:

	$ ssh -T git@github.com

### Configuring a Ruby/Coffeescript Project ###
1. Create a new project, and give it a name.
2. Under teh Source Code Management setting, enter in it's repository. I use the SSH git repo.
3. Under Build Triggers, if you want to poll your repository every 5 minutes, enter: `5 * * * *`
4. Set the RVM enviroment: `ruby-1.9.2-p320@project`
5. Configure the build.

Configuring the build is a customized process. I will include my build scripts so that you can base yours off of them.

	# Setup
	export QMAKE=/usr/bin/qmake-qt47
	rm -rf ${WORKSPACE}/db/development.sqlite3
	rm -rf ${WORKSPACE}/db/test.sqlite3
	rm -rf ${WORKSPACE}/db/production.sqlite3
	ln -fs /home/USER/Documents/PROJECT/development.sqlite3 ${WORKSPACE}/db/development.sqlite3
	ln -fs /home/USER/Documents/PROJECT/development.sqlite3 ${WORKSPACE}/db/test.sqlite3
	ln -fs /home/USER/Documents/PROJECT/development.sqlite3 ${WORKSPACE}/db/production.sqlite3
	bundle install

	#Run Jasmine and Rspec
	bundle exec jasmine init
	bundle exec rake jasmine:headless
	bundle exec rspec

	#Setup Cucumber and Run
	/etc/init.d/xvfb start
	export DISPLAY=:99.0 && bundle exec cucumber

Note: If you plan on running Cucumber tests, you need to install [xvfb](http://en.wikipedia.org/wiki/Xvfb) and modify your `init.d`.

	$ yum install xvfb

{% codeblock /etc/init.d lang:bash %}
#!/bin/bash
#chkconfig: 345 95 50
#description: Starts xvfb on display 99
if [ -z "$1" ]; then
echo "`basename $0` {start|stop}"
	exit
fi

case "$1" in
start)
	/usr/bin/Xvfb :99 -screen 0 1280x1024x24 &
;;

stop)
	killall Xvfb
;;
esac 
{% endcodeblock %}

### Wrap Up ###
My personal recommendation is to have multiple builds in Jenkins, instead of one large build. We currently have our jasmine/rspec suite checking for new commits every 5 minutes, while our cucumber build checks for new commits every hour. This helps to provide quick feedback to the developers, and prevents them from having to wait for Cucumber to finish running.

If you use this guide and run into any issues, leave a comment and I'll try to keep this guide updated. I wrote this guide about a week after I setup Jenkins, so it is possible I left out some steps.