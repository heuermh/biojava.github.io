---
title: BioJava:1.5ReleasePlan
---

Release plan for BioJava 1.5
============================

### Background

We would like to begin work on making a 1.5 release of BioJava. This
will include all new developments such as the BioJavaX and structure
APIs.

I propose we initially make a BioJava1.5 beta which will be a snapshot
of CVS and will only contain the documentation, demos and unit tests at
that date (not a complete and up to date suite).

A full BioJava 1.5 final release will ideally contain fully updated
documentation, demos, unit tests etc.

### Status

In planning phase. A [release Czar](Czar "wikilink") is being sought to
coordinate the current release.

### Alpha, beta, RCs

The following documents the proposed steps taken before a major release.
The Alpha release step should be considered optional.

#### Alpha

We don't normally make Alpha releases, the closest approximation would
be a snapshot of the CVS repository leading up to the beta release. An
Alpha release could be a CVS branch that serves as a proof of concept.
If the concept is accepted the branch may become the main trunk of the
CVS.

Requirements:

-   Code fully compiles under Ant.
-   Announcement made on biojava-dev list.

#### Beta

A Beta release would show the likely API of a final release.

Requirements:

-   Code fully compiles and passes JUnit tests.
-   All javadocs build and no warnings issued.
-   All demos and cookbook demos compile.
-   JARs, JavaDocs and source code posted to webserver (admin task).
-   Links to download and API updated (admin task).
-   Beta released announced on mail-list and news site.

#### Release Candidate

A release candidate is a possible final release. If no bugs are noted
within a certain testing time frame it could become a final release.

Requirements:

-   Has had a beta release.
-   Demo code, tutorials and cookbook examples updated to reflect best
    practices introduced new APIs and tested.
-   Where totally new functionality is introduced new cookbook, demo, or
    tutorial examples should be added.
-   New API's should have complete javadocs.
-   New API's should be marked with proper @since tags.
-   New API's should have good JUnit test coverage.
-   JARs, JavaDocs and source code posted to webserver (admin task).
-   Links to download and API updated (admin task).
-   Checks for backwards compatability are made.
-   Known errors and deficiencies documented.
-   RC released announced on mail-list and news site.
-   Time frame for final release decided and announced.

#### Final Release

A final release is a release candidate that has exceeded a period of
time with no new bugs detected.

Pre-release tasks
-----------------

Before BioJava1.5 can be released we need to consider the following
tasks. Please feel free to add more if you think of them. Things that
are not critical but nice to have should go in the wish-list section for
consideration.

### Coding

-   Code for any release (even Alpha) should minimally compile and pass
    all JUnit tests!

#### Changes to build.xml

-   add tasks to Ant build script to make distribution that includes
    biojava.jar, bytecode jar etc, all javadocs and docbook HTML (as
    zipped tar), and all source (as zipped tar).

<!-- -->

-   would be nice to have checksums for biojava.jar.

### Documentation

-   Update [Cookbook](BioJava:Cookbook "wikilink") code to reflect best
    practices with BioJavaX
-   Should we keep legacy examples in the
    [Cookbook](BioJava:Cookbook "wikilink")?
-   Check for errors in biojavax docbook

### Javadoc

-   The ant javadoc-all task must complete without any failures or
    warnings.
-   Volunteers are needed to check for poorly javadoced packages and add
    comments where they can.

### Quality

-   All JUnit tests must pass.
-   Volunteers needed to increase coverage of JUnit tests.
-   We badly need JUnit tests for BioJavaX / BioSQL interaction
-   Can someone with a good testing tool generate a coverage report?

### Check compatibility

#### Are RichSequence objects compatable with GUI code?

-   We need a volunteer to test how well RichSequence objects behave
    with biojava's GUI code.
-   GUI code as well as relevant javadocs, demos, and cookbook code may
    need to change.

#### Are BioJavaX objects compatable with DAS/DAZZLE?

Someone with experience of the DAS server DAZZLE is needed to check if
there are any issues with DAS and BioJavaX objects. This may not be at
all relevant but it would pay to check.

#### Backwards Compatibility

-   Are there any breaks in the API between biojava 1.4 and biojava 1.5?
-   Can someone run a change tool that will detect API differences that
    would prevent biojava 1.4 apps compiling with biojava 1.5

Wish-list, or Items-yet-to-be-sorted
------------------------------------

Edit this section with items to be considered for the 1.5 release

Reference
---------

The Apache Jakarta Commons project release prep and release notes might
contain helpful information, particularly as far as providing checksums
for and signing releases:

-   [Jakarta Commons - Preparations for a
    Release](http://jakarta.apache.org/commons/releases/prepare.html)
-   [Jakarta Commons - Cutting the
    Release](http://jakarta.apache.org/commons/releases/release.html)

