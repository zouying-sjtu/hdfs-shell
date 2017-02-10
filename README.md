
#HDFS Shell (tool)
HDFS Shell is a HDFS manipulation tool to work with functions integrated in Hadoop DFS


![Image of HDFS-Shell](https://github.com/avast/hdfs-shell/blob/master/web/screencast.gif)

## Purpose

There are 3 possible usecases:

- Running user interactive UI shell, inserting command by user
- Launching Shell with specific HDFS command
- Running in daemon mode - communication using UNIX domain sockets

###  Why such UI shell?

#### Advantages UI against direct calling hdfs dfs function:

- HDFS DFS initiates JVM for each command call, HDFS Shell does it only once - which means great speed enhancement when you need to work with HDFS more often
- Commands can be used in short way - eg. ```hdfs dfs -ls /```, ```ls /``` - both will work
- **HDFS path completion using TAB key**
- we can easily add any other HDFS manipulation function
- there is a command history persisting in history log (~/.hdfs-shell/hdfs-shell.log)
- **support for relative directory + commands ```cd``` and ```pwd```**

#### Disadvantages UI against direct calling hdfs dfs function:

- commands cannot be piped, eg: calling ```ls /analytics | less ``` is not possible at this time, you have to use HDFS Shell in Daemon mode

## Using HDFS Shell UI

### Launching HDFS Shell UI
#### Requirements:
- JDK 1.8
- It's working on both Windows/Linux Hadoop 2.6.0

#### Download
- [Download binary] (https://github.com/avast/hdfs-shell/releases/download/v1.0.0/hdfs-shell-1.0.0.zip)

#### Configuring launch scripts
HDFS-Shell is a standard Java application. For its launch you need to define 2 things on your classpath:
- all ```./lib/*.jar``` on classpath
- path to directory with your Hadoop config files (hdfs-site.xml, core-site.xml etc.), on Linux it's usually located in /etc/hadoop/conf folder - without these files the HDFS Shell will work in local filesystem mode

Pre-defined launch scripts are located in the zip file. You can modify it locally as needed.

- for CLI UI run ```hdfs-shell.sh``` (without parameters) otherwise:
- HDFS Shell can be launched directly with the command to execute - after completion, hdfs-shell will exit
- launch HDFS with ```hdfs-shell.sh script <file_path>``` to execute commands from file

#### Possible commands inside shell

- type ```help``` to get list of all supported commands
- ```clear``` or ```cls``` to clear screen
- ```exit``` or ```quit``` or just ```q``` to exit the shell
- for calling system command type ```! <command>``` , eg. ```! echo hello``` will call the system command echo
- type (hdfs) command only without any parameters to get its parameter description, eg. ```ls``` only
- ```script <file_path>``` to execute commands from file

### Running Daemon mode
![Image of HDFS-Shell](https://github.com/avast/hdfs-shell/blob/master/web/screenshot2.png)

- run hdfs-shell-daemon.sh
- then communicate with this daemon using UNIX domain sockets - eg. ```echo ls /analytics | nc -U /var/tmp/hdfs-shell.sock```

##Project programming info
The project is using Gradle 3.x to build. By default it's using Hadoop 2.6.0, but it also has been succesfully tested with with version 2.7.x. 
It's based on [Spring Shell](https://github.com/spring-projects/spring-shell) (includes JLine component). Using Spring Shell mechanism you can easily add your own commands into HDFS Shell.
(see com.avast.server.hdfsshell.commands.ContextCommands or com.avast.server.hdfsshell.commands.HadoopDfsCommands for more details)

**All suggestions and merge requests are welcome.**

####Other tech info:
For developing, add to JVM args in your IDE launch config dialog: 
``` -Djline.WindowsTerminal.directConsole=false -Djline.terminal=jline.UnsupportedTerminal```

###Contact
Author&Maintainer: Ladislav Vitasek  - vitasek/@/avast.com
