---
title: AapropsSkype 20110602
---

Participants:
-------------

Ah Fu, Peter, Andreas

Ismb
----

Peter and Andreas will meet at Ismb Vienna

Junit tests
-----------

We start to have unit tests. They are testing correct usage and results
of the software. Peter: also add extreme cases for testing in an attempt
to try to break the API.

- Equals test for double: precision problem. Solution: round to the
desired precision. - Execution time of tests - broken tests - can't
install with Maven

Exceptions for invalid characters
---------------------------------

Shall we throw exceptions for invalid characters or just ignore them?

If we can fix the problem, we will try to fix it, otherwise we will
throw exception.

Javadocs in SVN
---------------

ignore in SVN

Outlook for next week
---------------------

break API as part of junit tests

start working on next properties: solv access sec struc charge
hydrophobicity

molecular weight - load from XML to support multiple source
