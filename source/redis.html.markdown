---
title: Redis
---

> **Links:** [Homepage](http://redis.io/) | [Downloads](http://redis.io/download) | [Documentation](http://redis.io/documentation)  
> **Dependencies:** None  
> **Version:** <span id="version">4.0.2</span>

**Redis** is an open source, BSD licensed, advanced key-value store.


### Get the Code

Switch to `/usr/local/src` and download the source package.

	cd /usr/local/src
	curl --remote-name http://download.redis.io/releases/redis-VERSION.tar.gz

Extract the archive and move into the folder.

	tar -xzvf redis-VERSION.tar.gz
	cd redis-VERSION


### Compile and Install

Configure, compile and install into `/usr/local/mac-dev-env/redis-VERSION`.

	make
	make PREFIX=/usr/local/mac-dev-env/redis-VERSION install

Create a symbolic link to `/usr/local/redis`.

	sudo ln -s mac-dev-env/redis-VERSION /usr/local/redis


### Shell

Execute the following lines to update your [Bash](http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) startup script.

	echo 'export PATH=/usr/local/redis/bin:$PATH' >> ~/.bash_profile

Load the new shell configurations.

	source ~/.bash_profile


### Database

Create a folder that will contain your database. My database is located in `/usr/local/var/redis`. You can place your database wherever you'd like but make sure you update the path when mentioned throughout this article.

	mkdir -p /usr/local/var/redis


### Configuration File

Create a configuration file so you can make changes to your configuration without messing around with command line arguments.

	nano /usr/local/redis/redis.conf

Copy and paste the following text into the aforementioned file.

	# Data folder
	dir /usr/local/var/redis
	# Bind to localhost
	bind 127.0.0.1


### Manual Start/Stop

To start the Redis server.

	redis-server /usr/local/redis/redis.conf

Press `CTRL-C` to stop the Redis server.


### Automatically Start the Server at Boot

Create a configuration file for [Launchd](http://en.wikipedia.org/wiki/Launchd).

	nano ~/Library/LaunchAgents/io.redis.redis-server.plist

Copy and paste the following text into the aforementioned file.

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	  <dict>
	    <key>Label</key>
	    <string>io.redis.redis-server</string>

	    <key>ProgramArguments</key>
	    <array>
	      <string>/usr/local/redis/bin/redis-server</string>
	      <string>/usr/local/redis/redis.conf</string>
	    </array>

	    <key>StandardOutPath</key>
	    <string>/usr/local/var/log/redis.log</string>
	    <key>StandardErrorPath</key>
	    <string>/usr/local/var/log/redis.log</string>

	    <key>RunAtLoad</key>
	    <true/>
	    <key>KeepAlive</key>
	    <true/>
	  </dict>
	</plist>

Register with Launchd and start the server.

	launchctl load ~/Library/LaunchAgents/io.redis.redis-server.plist

Deregister with Launchd. Kill the process manually.

	launchctl unload ~/Library/LaunchAgents/io.redis.redis-server.plist


### Verify the Installation

Verify that you have successfully installed **Redis**.

	redis-server -v


### Testing Redis

You can connect to the Redis server using the command line utility.

	redis-cli

To test if everything is working correctly.

	ping

To set a cache item.

	set foo hello

To retrieve the cache item.

	get foo

To exit the session.

	quit

For more information on using [Redis commands](http://redis.io/commands).
