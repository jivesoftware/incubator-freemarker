Jive-customized Freemarker Notes

To build locally:
-----------------

* brew install ant
* brew install ivy
* mkdir -p ~/.ant/lib
* cp /usr/local/Cellar/ivy/2.4.0/libexec/ivy-2.4.0.jar ~/.ant/lib
* (from root dir) ant 
==> freemarker.jar is placed in 'build' directory
6. install jar in local repository via:

	mvn install:install-file -Dfile=build/freemarker.jar -DgroupId=org.freemarker -DartifactId=freemarker -Dversion=2.3.22-jive-1 -Dpackaging=jar


CI build:
-----------------

1. build via Ant build:

	ant -f build.xml

2. install Ant-built jar to Nexus using Maven via:

	mvn deploy:deploy-file -DgroupId=org.freemarker \
      -DartifactId=freemarker \
      -Dversion=2.3.22-jive-1 \
      -Dpackaging=jar \
      -Dfile=build/freemarker.jar \
      -DrepositoryId=modified-thirdparty \
      -Durl=http://nexus-int.eng.jiveland.com/content/repositories/modified-thirdparty


2.3.22 Upgrade 
-----------------
JIVE-21020 don't allow arbitrary code execution through custom FreeMarker template
APPLIED: made Execute.exec() a noop (security issue)


2.3.21 Upgrade (from 2.3.15) (see JIVE-50429)
---------------------------------------------------

JIVE-25064 Upgrade guava to v14
N/A

JIVE-21020 don't allow arbitrary code execution through custom FreeMarker template
APPLIED: made Execute.exec() a noop (security issue)

MAS-807 freemarker.core.Environment perf fixes for highly concurrent contention issues
NOT APPLIED: Caching was introduced later. By not applying this patch it also negates the need for introducing crusty version
			 of Guava with now deprecated MapMaker.softValues().

CS-10273 Freemarker upgraded to 2.3.15
N/A

CS-10121 Freemarker uses too much flow control by exception
NOT APPLIED: The affected code now includes similar performance improvements.

CS-4917 Reduce thread contention in freemarker common paths through the use of thread safe maps and other concurrent data structures.
NOT APPLIED: The code changed significantly and now includes more thread safe data structures.


---

OLD NOTES (2.3.15-jive and older):

#1. Checkout https://freemarker.svn.sourceforge.net/svnroot/freemarker/branches/2.3/freemarker
#2. Copy files (diffing to verify changes) from this repository to the checked out repository
#3. Build freemarker.

CS-4917
CS-6156
CS-10121
JIVE-21020
JIVE-25064 (removing usage of softKeys)


Additional notes on building

build.xml changes

	- Build with java 1.6
	- Remove bootclasspath parameter from javac rule so concurrent classes can be found
	- Add guava jar for MapMaker class

After checkout, do an initial build before making any source file changes

	$ ant

This allows the FMParser.java file to be generated.  Changes can now be overlayed and the jar file can be built.
