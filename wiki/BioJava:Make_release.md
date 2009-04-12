---
title: BioJava:Make release
---

How to make a BioJava release
-----------------------------

This page is intended for BioJava release managers. I was documenting
this while I was doing the BioJava 1.7
release. --[Andreas](User:Andreas "wikilink") 15:14, 12 April 2009 (UTC)

### Required time

A few hours. Most time is being spent in verifying that the code base is
release ready. The actual preparation of the .jar files and copying them
to the open-bio.org server is quite quick and can be done
semi-automatic.

### Prior to release

-   Announce release deadlines on mailing list

### On release date

**Verify code base**

-   Make sure code is ready for release. Check last minute commits
    (there usually are some).
-   Make sure the auto-build page (cruisecontrol) does not report any
    problems
-   Run

`ant-clean; ant runtests;`

-   make sure there are no broken tests being reported.
-   fix anything that needs to be fixed prior to release.

If this all fine up to here, we are ready for release.

**Branch and Tag SVN**

-   Branch and tag the svn directories. svn copy the trunk to the
    corresponding directories in the /branches and /tags directories of
    the svn repository.

`svn cp -m "branching 1.7 release" svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/trunk svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/branches/release-1_7-branch`  
`svn cp -m "branching 1.7 release" svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/trunk svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/tags/release-1_7`

-   Verify that all went well

`svn list svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/branches`  
`svn list svn+ssh://dev.open-bio.org/home/svn-repositories/biojava/biojava-live/tags`

**Build the release**

-   edit the build.xml file and change the **version** field to the
    number of the current release.
-   Run

`ant-clean; ant dist;`

-   check that the javadocs have been built ok, first page should show
    the number of the current release/

`open `[`file:///path/to/your/local/dir/biojava-live/dist/biojava-1.7/doc/biojava/index.html`](file:///path/to/your/local/dir/biojava-live/dist/biojava-1.7/doc/biojava/index.html)

-   prepare the biojava-all.jar bundle

`cd dist; jar cvf biojava-1.7-all.jar biojava-1.7`

### Copy files to portal.open-bio.org

-   Log into the portal.open-bio.org server. go to
    /home/websites/biojava.org/html/static/download/ and create a new
    directory for the new release. See directory structure in other
    release how to organise this.

<!-- -->

-   Copy file biojava-all.jar

`scp biojava-1.7-all.jar username@portal.open-bio.org:/home/websites/biojava.org/html/static/download/bj17/all`

**Back in your local checkout**

-   remove the /dist subdirectory from your checkout. Not needed any
    longer.
-   Make the doc release bundle

`ant clean; ant javadocs-all; cd ant-build; jar cvf biojava-1.7-doc.jar docs/`

-   Copy file biojava-doc.jar

`scp biojava-1.7-doc.jar username@portal.open-bio.org:/home/websites/biojava.org/html/static/download/bj17/doc/biojava-docs.jar`

-   Prepare the bin release:

`ant clean; ant;`

-   Copy file biojava.jar

`scp ant-build/biojava.jar username@portal.open-bio.org:/home/websites/biojava.org/html/static/download/bj17/bin/`

-   Prepare the src release:

`ant clean; cd .. ; jar cvf biojava-1.7-src.jar biojava-live`  
`scp biojava-1.7-src.jar username@portal.open-bio.org:/home/websites/biojava.org/html/static/download/bj17/src/`

### Update Javadoc docu on portal.open-bio.org

Almost done... The last thing is to update the public javadoc files. For
this, we need to recompile the javadocs and add a hook to the google
analytics tracker, we are using on the website. use the modified
*my\_build\_biojava.xml* that is attached here. (update the version
variable in the file to your release number)

`ant -buildfile my_build_biojava.xml javadocs-biojava`

Then copy the newly created javadoc files to the public server.

`cd ant-build/docs/; tar cvf javadocs-server.tar ./biojava/`  
`scp javadocs-server.tar username@portal.open-bio.org:~/`

The files are now in your home directory on the server. Untar the files
on portal.open-bio.org:

`tar xvf javadocs-server.tar`

The files are now in your home directory on the server. We need to hook
them into the frontend:

`cd /home/websites/biojava.org/html/static/docs/`  
`mkdir api17`  
`cp -r ~/biojava/* ./api17/ `

verify that all went ok by pointing your browser to

[`http://www.biojava.org/docs/api17/`](http://www.biojava.org/docs/api17/)

update the symbolic link to the javadoc api

`point /home/websites/biojava.org/html/static/docs/api to api17`

### Update the wikipedia pages to link to the new release

Create a new download file for the release. (I copied
<BioJava:Download_1.6> to <BioJava:Download_1.7>. Modify the new page to
the latest data.

Update <BioJava:Download> (Change the redirect on the BioJava:Download
page to <BioJava:Download_1.7>)

Update the <MediaWiki:Sidebar> to point to the new Javadoc api

### AND FINALLY

Write release announcement to biojava-l and biojava-dev