---
layout: post
title:  "Install SonarQube"
date:   2017-07-13 15:38:00
categories: education
comments: true
---

Because the main aim of GSoC is to make every participant a better engineer, we have been advised to use additional tools which examine the quality of the codebase we are working on. Moreover, they can help us to compare the project before and after GSoC. One of them is SonarQube. I have heard of it during my Bachelor but I have never taken seriously such tools. Anyway, in this blog post, I will try to make a really quick tutorial how to install SonarQube. There exists official documentation (links provided below), but I will try to point out the steps I was confused about and I messed up a little bit :flushed:

### Environment

* OS X El Captain 10.11.6
* Java 8
* MySQL Server 5.6

### Getting started
The first step is to download the [sonarqube zip](https://www.sonarqube.org/#downloads) and extract it somewhere on your computer.
> :heavy_exclamation_mark: The step that I forgot here was to create a DB scheme.

The installation guide is a little bit confusing especially if you start with “Getting started for two minutes” tutorial. So let's not skip this step and create a scheme which our sonarQube server will use to store information for our projects. I also created a new user so that it can access only this scheme. Of course, you can use already existing user if you prefer. <br />
The MySQL script looks like that:

```
# create the scheme
# default charset is UTF-8 - so no need to change it
CREATE SCHEMA IF NOT EXISTS ‘sonarqube‘;
# create new user with name sonarqube and password pass - DO NOT FORGET TO CHANGE THE PASSWORD
# The sonarqube account can be used only when connecting from the localhost
CREATE USER ‘sonarqube‘@‘localhost‘ IDENTIFIED BY ‘pass’;
# granting access only to sonarqube database
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON sonarqube.* TO ‘sonarqube‘@‘localhost‘;
```

After the DB and the user are successfully created. The next step is to update properties of sonarqube.
Therefore, go to the folder where you extracted sonarqube. In the **/conf** folder, you will find **sonar.properties** file. This file contains properties of your sonarqube server. You have to set your DB properties:
```
# the username and the password you have created on the previous step
sonar.jdbc.username=sonarqube
sonar.jdbc.password=your_password
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonarqube?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
```
These three properties are mandatory and enough for a quick start. I have also updated `sonar.web.port` to 9999 because my wildfly server was running on the sonarqube's default port 9000. But you can skip this step if you do not have anything running on this port. <br/>

> :heavy_exclamation_mark: One thing to mention is that in the **[sonarQubePath]/logs** folder you can find the logs. If something is not running correctly, please examine these files. You have one for elastic search (es.log), one for your web server (web.log) and one for your sonarqube (sonar.log).

<p>So now if you run this command and everything was set up successfully </p>
```
[pathToSonarQube]/bin/[OS]/sonar.sh console
```
your server will be available on the address `http:\\localhost:9999` (or `http:\\localhost:9000` if you haven't changed the port). Congrats, your sonarqube server is installed and running :clap: :clap: :clap:

Although, we have installed sonarqube, to analyse a project, we need sonar scanner. Firstly, you download [the zip](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) and extract it to a folder somewhere on your computer. Then you have to update your **[sonarScannerPath]/conf/sonar-scanner.properties** file. The only thing I have done was to uncomment `sonar.host.url` property and change the port, so it looks like that:
```
sonar.host.url=http://localhost:9999
```
> :exclamation: If you are using the default sonarqube port, your address will be http://localhost:9000


Then you have to add your **[sonarScannerPath]/bin** folder to your path. It can be done easily by executing this command:
```
export PATH=$PATH:[sonarScannerPath]/bin
```

Be sure that you verify your installation by executing the command:
```
sonar-scanner -h
```
If it gives you unknown bash command then you haven't added the **[sonarScannerPath]/bin** successfully to your path.
> Take a breath, we are almost ready.

The final thing is to add **sonar-project.properties** file to the project which you want to analyse.
A sample of a sonar-project.properties file looks like that:
```
# must be unique in a given SonarQube instance
sonar.projectKey=aerogearups
# this is the name and version displayed in the SonarQube UI. Was mandatory prior to SonarQube 6.1.
sonar.projectName=Aerogear Unified Push Server
sonar.projectVersion=0.0.1

# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
# This property is optional if sonar.modules is set.
sonar.sources=.
# Encoding of the source code. Default is default system encoding
#sonar.sourceEncoding=UTF-8
```
Finally, we are ready to run our sonar scanner. Go to the root folder of your project and execute this command:
```
[sonarScannerPath]/bin/sonar-scanner
```

> :heavy_exclamation_mark:  I had problems using only sonar-scanner command but the full path worked for me.

UHUUUU, now you can see the analysis report of your project. I am still exploring [Aerogear Unified Push Server](https://aerogear.org/) - the project I am working on during GSoC. Can't wait to share its results :smiley:

Share your feedback and stay tuned for my next post. <br/>
**Do not overthink, be happy and just keep smiling...**
<br /> Polina

## References
[Get Started in Two Minutes](https://docs.sonarqube.org/display/SONAR/Get+Started+in+Two+Minutes)
<br/>[Installing the Server](https://docs.sonarqube.org/display/SONAR/Installing+the+Server)
