OpenJDK 8 development environment
=================================

OpenJDK 8 development environment packaged as a VirtualBox VM.<br />
OS: Ubuntu 12.04<br />
IDE: Eclipse Kepler 4.3<br />
Download location (~ 7 GB zipped archive): https://drive.google.com/file/d/127eWlbL-MBk65DiDTcwfc6mOzTHjcOxC<br />
OpenJDK user password: j1a2v3a4

# Basic information

Local OpenJDK repo is cloned in /home/OpenJDK/dev/jdk8_tl.<br />
All of the relevant repos are here: hg.openjdk.java.net.<br />
The eclipse workspace is located under /home/openjdk/workspace.<br />

IMPORTANT: <br />
You should add your JDK user (in case you are given commiter rights and you have registered your JDK user- typically your java.net user ) 
to the Mercurial configuration in ~/dev/jdk8_tl/.hg/hgrc as follows: <br />
>[ui]<br />
>username=<OpenJDK_username><br />
>[extensions]<br />
>fetch=<br />

In case you open Eclipse and you see no projects you 
can import them from the /home/openjdk/dev/jdk8_tl folder (using 
the 'nested projects' option).

# Contribution

To start contributing to OpenJDK you can follow this process:<br />
1) Sign OCA (Oracle Contributor Agreement) – specify OpenJDK as the the project and your java.net user as the username<br />
2) Send the signed OCA to oracle-ca_us@oracle.com<br />
3) Find some interesting bug or enhancement (RFE) to work on from bugs.sun.com<br />
4) You may subscribe to a particular mailing list of interest<br />
5) Discuss any changes you want to make in the appropriate mailing list using the format:<br />

>&lt;Bug_or_RFE_Id&gt; : &lt;Bug_or_RFE_Title&gt; <br />

6) Add a proposed code change (patch) to the discussion using any of the following commands:<br />

>hg export -g<br />
>hg diff -g<br />

7) If applicable attach JTReg tests to the suggested changeset <br /><br />

You may additionally install the 'hgview' that provides a UI for navigating the Mercurial repo:<br />
>sudo apt-get install hgview<br />

You also may want to enable the 'jcheck' tool in your local repository so that you 
never create, pull, or import invalid changesets - see http://openjdk.java.net/projects/code-tools/jcheck/.<br />


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


