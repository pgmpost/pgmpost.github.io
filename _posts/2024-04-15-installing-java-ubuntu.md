---
layout: post
title: Installing Java development environment on Ubuntu 20.04
---

**Content**

- [Introduction](#introduction)
- [Installing SDKMAN](#installing-sdkman)
- [Installing Java](#installing-java)
- [Installing Gradle](#installing-gradle)
- [Installing IntelliJ IDEA](#installing-intellij-idea)
- [Configuring IntelliJ IDEA](#configuring-intellij-idea)

<br>

### Introduction

Java is one of the most used programming language on the planet, it can run on many platforms and you can build pretty much anything with it. From web applications, embeded services to distributed applications...

Ubuntu being the most popular linux distribution, knowing how to set up a Java dev environement on Uubntu is essential :)

If you don't have Ubuntu installed, you can [follow our tutorial](https://pgmpost.github.io/2024/04/19/installing-configuring-ubuntu-20.04.html)

First thing to do is update our system.

Fire-up a terminal and enter the following commands :

	sudo apt-get update
	sudo apt-get upgrade

<br>

### Installing sdkman

Type in the terminal the following commands and follow the instructions :

	curl -s "https://get.sdkman.io" | bash
	source "$HOME/.sdkman/bin/sdkman-init.sh"

Then confirm sdkman is properly installed, type in :

	sdk version

<br>

### Installing Java

Listing java available candidates gives us a long list :

	sdk list java

![sdk-list](/pictures/sdk-list.png)


To install java 22 open jdk, just type in :

	sdk install java 22-open

Follow the instructions if any...

To install jdk11, type in :

	sdk install java 11.0.14.1-jbr

If you want to change the java version from 22 to 11, type in :

	sdk default java 11.0.14.1-jbr

Lastly we set the JAVA_HOME variable to the current java version :

	export JAVA_HOME='$SDKMAN_DIR/candidates/java/current'

<br>

### Installing Gradle

To install gradle we proceed in the same manner :

	sdk list gradle

Choose the version you want from the list and install it with sdk :

	sdk install gradle 8.7

<br>

### Installing intelliJ IDEA

Got to intelliJ IDEA website and download the version you want.

Extract the file :

	 sudo tar -xzf ideaIC-*.tar.gz -C /opt

In the bin directory execute the idea.sh script :

	sudo bash idea.sh

The create a desktop entry, click the gear at the bottom left corner and select *create desktop entry*.

![desktop-entry](/pictures/desktop-entry.png)

In the *Applications* view in ubuntu, locate IntelliJ IDEA and add it the the favorite so it can appear on the left panel.

<br>

### Configuring IntelliJ IDEA


**Configuring gradle**

Select File > Settings > Build, Execution, Deployment > Build tools > Gradle.

Specify gradle user home as :

	path_to_home/.gradle

In the Gradle projects section, select use local distribution and set it to :

	path_to_home/.sdkman/candidates/gradle/8.7
	
And the Gradle JVM from the list of detected SDK to :

	path_to_home/.sdkman/candidates/java/current

where path_to_home is the path to your *home* directory

Hit *Apply* then *Ok*.


**Configuring the Java SDK**

Select File > Project Structure > Project Settings, then set the SDK to *current* from the detected sdks.

Under Platform Settings > SDKs, make sure the current sdk is added or add it with the + sign.

Hit *Apply* then *Ok* and you're ready to go.

<br>

### Final Remarks

Some people are reporting that although gradle is set to be used from a local distribution, the editor still downloads a gradle version for each new project. Don't know how to fix that and apparently it is a bug.

Leave a comment below to report anything form your part.











