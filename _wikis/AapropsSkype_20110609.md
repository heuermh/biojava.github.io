---
title: AapropsSkype 20110609
---

Participants:
-------------

Ah Fu, Peter, Andreas

Junit tests
-----------

last week not all tests passed,that's fixed. Also testing of extreme
values was added.

Peter: don't catch (expected) exceptions in junit tests, use annotations
instead.

rounding: probably the best approach is to use String.format() or
DecimalFormat.

XML parsing
-----------

some difficulties: What is the best approach about doing this?

- first approach was use XMLspy to generate Schema from XML file.

- Peter: better to create the class first and then auto-generate XML
from that.

- Start with a new elements class that captures the elements in the
periodic table

- Use Jaxb to export to XML

- Create loader

- later add a compound class

- make sure to add side-chains to amino acids

Peter will send out an example of how he thinks this should be done

Outlook for this week
---------------------

`- XML representation of Elements`

`- finish up junit tests`
