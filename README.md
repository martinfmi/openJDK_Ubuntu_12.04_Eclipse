OpenJDK 8 development environment
=================================

OpenJDK 8 development environment packaged as a VirtualBox VM.<br />
OS: Ubuntu 12.04<br />
IDE: Eclipse Kepler 4.3<br />
Download location (~ 7 GB zipped archive): https://docs.google.com/file/d/0B6c0vUgNZve1M0ZOMWZxM3BsdEE<br />
OpenJDK user password: j1a2v3a4

# Basic information

Local OpenJDK repo is cloned in /home/OpenJDK/dev/jdk8_tl.<br />
All of the relevant repos are here: hg.openjdk.java.net.<br />
The eclipse workspace is located under /home/openjdk/workspace.<br />

IMPORTANT: In case you open Eclipse and you see no projects you 
can import them from the /home/openjdk/dev/jdk8_tl folder (using 
the 'nested projects' option).

# Build

>sudo bash configure - in order to configure the repo locally issue<br />
>ccache -M 3G - this is optional but if issued uses caching to improve incremental building<br />
>make clean images - build all projects<br />
>make <project_name>-only - build a particular project<br /> 
>			(e.g. make jdk-only)<br />
			
Add the JDK_FILTER="[classes]" option to limit the classes to recompile in the jdk project.<br />

# Projects

All of the projects can be built by issuing Run As -> Ant Build on the corresponding project.<br />
Only hotspot is being build using external 'make' configuration. To build it ussue: Project -> Build Project.<br />

## Swing

Command line build: ant clean build -propertyfile build.properties [&> swingAntBuild.log]<br />
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/swing.jar<br />

## JMX

Command line build: ant clean build -propertyfile build.properties [&> jmxAntBuild.log]<br />
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/jmx.jar<br />

## JConsole

Command line build: ant clean build -propertyfile build.properties [&> jconsoleAntBuild.log]<br />
Deployment artifacts: ~/dev/jdk8_tl/jdk/dist/lib/jconsole.jar<br />

## JAXP/JAXWS

Command line build: cd ~/dev/jdk8_tl/common/makefiles <br />

>make clean jaxp NEWBUILD=true [&> jaxpInfrabuild.log]<br />
>(make clean jaxws NEWBUILD=true [&> jaxwsInfrabuild.log])<br />

Deployment artifacts: <br />
~/dev/jdk8_tl/build/linux-x86_64-normal-server-release/jaxp<br />

>classes/<br />
>dist/<br />

~/sources/jdk8_tl/build/linux-x86_64-normal-server-release/jaxws<br />

>dist/<br />
>jaf_classes/<br />
>jaxws_classes/<br />

## langtools

Command line build: cd ~/dev/jdk8_tl/common/makefiles<br />

>make clean langtools NEWBUILD=true [&> langtoolsInfrabuild.log]<br />

Deployment artifacts:<br />
~/dev/jdk8_tl/build/linux-x86_64-normal-server-release/langtools<br />

>btclasses/<br />
>classes/<br />
>dist/<br />
>gensrc/<br />
>genstubs/<br />

## hotspot

(optional) If you need to configure your hotspot environment issue:<br />

>	cd ~/dev/jdk8_tl//common/makefiles<br />
>	bash ../autoconf/configure --with-boot-jdk=/usr/lib/jvm/java-7-openjdk-amd64/<br />
>	bash ../autoconf/configure --with-boot-jdk=/usr/lib/jvm/java-7-openjdk-amd64/ JAVAC_FLAGS=-g<br />
>			(to enable generation of debug classfiles)<br />

Command line build:<br />

>	make clean hotspot NEW_BUILD=true [&> hotspotInfraBuild.log]<br />
>	make hotspot NEW_BUILD=true [&> hotspotInfraBuild.log]<br />
>				(incremental build)<br />
>	make hotspot NEW_BUILD=true DEBUG_CLASSFILES=true &> hotspotInfraBuild-DEBUG_CLASSFILES.log<br />
>				(for generating debug classfiles when building)<br />

Deployment artifacts:<br />
The general Hotspot build artefacts are a tree of directories representing the different kinds of builds that can occur e.g.:<br />

>	linux_i586_compiler1 linux x86 client compiler build<br />
>	linux_i586_compiler2 linux x86 server compiler build<br />
>	linux_amd64_compiler1 linux amd64 client compiler build<br />
>	linux_amd64_compiler2 linux amd64 server compiler build<br />

Those directories then further split into:<br />

>	debug<br />
>	fastdebug<br />
>	generated<br />
>	jvmg<br />
>	optimized<br />
>	product<br />
>	profiles<br />

Tests
=====

To run a particular test suite run from the 'tests' directory:<br />
>make <package> // e.g. make jdk_util<br />

or<br />

>make TESTS="<package_1> ... <package_N>"<br />

You can run a particular test using jtreg. For example:<br />

>cd $HOME/dev/jdk8_tl/jdk/test<br />
>$HOME/dev/jtreg -verbose:fail java/lang/invoke/AccessControlTest.java<br />

References
==========

The OpenJDK Developers' Guide<br />
	http://openjdk.java.net/guide/<br />
Adopt OpenJDK wiki<br />
	https://java.net/projects/adoptopenjdk/pages/AdoptOpenJDK<br />
Getting Started with HotSpot and OpenJDK<br />
	http://www.infoq.com/articles/Introduction-to-HotSpot<br />
OpenJDK Governance and Development Process Overview<br />
	http://www.youtube.com/watch?v=jebmrXo-Y3Y<br />
OpenJDK and Adopt OpenJDK<br />
	http://www.youtube.com/watch?v=GgoXqZgguyo<br />
Hacking the OpenJDK compiler<br />
	http://www.ahristov.com/tutorial/java-compiler.html<br />
How to compile openJDK under Ubuntu<br />
	www.vogella.com/articles/OpenJDK/article.html<br />
Meet the OpenJDK Tests<br />
	http://www.youtube.com/watch?v=ZcOGof_DXcU<br />
OpenJDK Testing Pitfalls presentation by Stuart W.Marks<br />
	http://www.youtube.com/watch?v=zKUI5HJVqCs<br />
Hacking Hotspot in Eclipse<br />
	http://rkennke.wordpress.com/2012/07/27/hacking-hotspot-in-eclipse/<br />
HotSpot Internals<br />	
	https://wikis.oracle.com/display/HotSpotInternals/Home<br />


