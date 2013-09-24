OpenJDK 8 development environment
=================================

OpenJDK 8 development environment packaged as a VirtualBox VM.
OS: Ubuntu 12.04
IDE: Eclipse Kepler 4.3
Download location (~ 7 GB zipped archive): https://docs.google.com/file/d/0B6c0vUgNZve1M0ZOMWZxM3BsdEE

// ************************************
// This file contains information on 
// developing on top of the OpenJDK 
// using Eclipse as the IDE of choice
// ************************************

Basic information
=================

Local OpenJDK repo is cloned in /home/OpenJDK/dev/jdk8_tl.
All of the relevant repos are here: hg.openjdk.java.net.
The eclipse workspace is located under /home/openjdk/workspace.

Build
=====

sudo bash configure - in order to configure the repo locally issue
ccache -M 3G - this is optional but if issued uses caching to improve incremental building
make clean images - build all projects
make <project_name>-only - build a particular project 
			(e.g. make jdk-only)
			
Add the JDK_FILTER="[classes]" option to limit the classes to recompile in the jdk project.

Projects
========

All of the projects can be built by issuing Run As -> Ant Build on the corresponding project.
Only hotspot is being build using external 'make' configuration. To build it ussue: Project -> Build Project.

-- Swing -- 

Command line build: ant clean build -propertyfile build.properties [&> swingAntBuild.log]
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/swing.jar

-- JMX --

Command line build: ant clean build -propertyfile build.properties [&> jmxAntBuild.log]
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/jmx.jar

-- JConsole --

Command line build: ant clean build -propertyfile build.properties [&> jconsoleAntBuild.log]
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/jconsole.jar

-- JAXP/JAXWS -- 

Command line build: cd ~/dev/jdk8_tl/common/makefiles 
		make clean jaxp NEWBUILD=true [&> jaxpInfrabuild.log]
		(make clean jaxws NEWBUILD=true [&> jaxwsInfrabuild.log])
Deployment artifacts: ~/dev/jdk8_tl/build/linux-x86_64-normal-server-release/jaxp
			classes/
			dist/
		      ~/sources/jdk8_tl/build/linux-x86_64-normal-server-release/jaxws
			dist/
			jaf_classes/
			jaxws_classes/

-- langtools -- 

Command line build: cd ~/dev/jdk8_tl/common/makefiles
make clean langtools NEWBUILD=true [&> langtoolsInfrabuild.log]
Deployment artifacts:
	~/dev/jdk8_tl/build/linux-x86_64-normal-server-release/langtools
		btclasses/
 	   	classes/
    		dist/
    		gensrc/
    		genstubs/

-- hotspot --

(optional) If you need to configure your hotspot environment issue:
	cd ~/dev/jdk8_tl//common/makefiles 
	bash ../autoconf/configure --with-boot-jdk=/usr/lib/jvm/java-7-openjdk-amd64/
	bash ../autoconf/configure --with-boot-jdk=/usr/lib/jvm/java-7-openjdk-amd64/ JAVAC_FLAGS=-g
			(to enable generation of debug classfiles)

Command line build:
	make clean hotspot NEW_BUILD=true [&> hotspotInfraBuild.log]
	make hotspot NEW_BUILD=true [&> hotspotInfraBuild.log] 
				(incremental build)
	make hotspot NEW_BUILD=true DEBUG_CLASSFILES=true &> hotspotInfraBuild-DEBUG_CLASSFILES.log
				(for generating debug classfiles when building)
Deployment artifacts:
The general Hotspot build artefacts are a tree of directories representing the different kinds of builds that can occur e.g.:
	linux_i586_compiler1 linux x86 client compiler build
	linux_i586_compiler2 linux x86 server compiler build
	linux_amd64_compiler1 linux amd64 client compiler build
	linux_amd64_compiler2 linux amd64 server compiler build
Those directories then further split into:
	debug
	fastdebug
	generated
	jvmg
	optimized
	product
	profiles

Tests
=====

To run a particular test suite run from the 'tests' directory:
	make <package> // e.g. make jdk_util
or
	make TESTS="<package_1> ... <package_N>"

You can run a particular test using jtreg. For example:

cd $HOME/dev/jdk8_tl/jdk/test
$HOME/dev/jtreg -verbose:fail java/lang/invoke/AccessControlTest.java

References
==========

The OpenJDK Developers' Guide
	http://openjdk.java.net/guide/
Adopt OpenJDK wiki
	https://java.net/projects/adoptopenjdk/pages/AdoptOpenJDK
Getting Started with HotSpot and OpenJDK
	http://www.infoq.com/articles/Introduction-to-HotSpot
OpenJDK Governance and Development Process Overview	
	http://www.youtube.com/watch?v=jebmrXo-Y3Y
OpenJDK and Adopt OpenJDK
	http://www.youtube.com/watch?v=GgoXqZgguyo
Hacking the OpenJDK compiler
	http://www.ahristov.com/tutorial/java-compiler.html
How to compile openJDK under Ubuntu
	www.vogella.com/articles/OpenJDK/article.html
Meet the OpenJDK Tests
	http://www.youtube.com/watch?v=ZcOGof_DXcU
OpenJDK Testing Pitfalls presentation by Stuart W.Marks
	http://www.youtube.com/watch?v=zKUI5HJVqCs
Hacking Hotspot in Eclipse
	http://rkennke.wordpress.com/2012/07/27/hacking-hotspot-in-eclipse/
HotSpot Internals	
	https://wikis.oracle.com/display/HotSpotInternals/Home


