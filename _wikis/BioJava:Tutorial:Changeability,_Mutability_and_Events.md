---
title: BioJava:Tutorial:Changeability, Mutability and Events
---

**By [Matthew Pocock](mailto:mrp@sanger.ac.uk)**

BioJava contains a powerful API for communicating when objects wish to
change their state, and potentialy preventing them from changing if it
would invalidate the state of another object, all without violating the
principals of encapsulation. The main classes are in the
`org.biojava.utils` package and include `Changeable`, `ChangeEvent`,
`ChangeListener`, `ChangeType` and `ChangeVetoException`. For full
descriptions of all the API used here, please consult the JavaDoc API
documentation ([latest biojava
1.8](http://www.biojava.org/docs/api1.8/)).

What is the difference between Changeability and Mutability?
------------------------------------------------------------

Many Java objects are mutable. That is, you can invoke methods that
change their state. The Collections API supplys mutable implementations
of the `List` interface. There is also a method
`Collections.immutableList(List l)` that returns a view of the
underlying list where the mutators throw exceptions. Through this view
object there is no way to edit the list. However, if the underlying list
is modified then the 'immutable' view will reflect this. That is,
although it is immutable, it is still changeable.

Things get even more complicated in the world of bioinformatics. Many
instances need to be mutable with respect to some clients and immutable
for others. Also, some processes rely on objects remaining constant
throughout. You can't perform a database search reliably if the database
is being modified. However, once the search is complete there is no
reason not to change the database. This transient immutability can't be
modeled using the design pattern used for the collections. The situation
above is complicated even further because while a search is going on,
every single sequence must be maintained in an uneditable state.
However, a search object realy doesn't want to go through the process of
modifying every single sequence object. This would be very ineficient.
Something more flexible is needed, and the *Changeability API* is it.

What is a ChangeEvent?
----------------------

`ChangeEvent` extends `java.util.EventObject` and adds the methods:

-   `getChange` - the new value
-   `getPrevious` - the old value
-   `getType` - the 'type' of event
-   `getChained` - an event that caused this event to be fired

In constrast to the classical Java events model, one event class is
shared among all types of BioJava events. The 'type' of the event is
signaled by the value of the `type` property. `ChangeType` is a final
class. Each interface that will fire `ChangeEvents` will have
`public static final ChangeType` fields with descriptive names.
ChangeEvent objects store a descriptive name but are always compared
with the `==` operator. This scheme is a type-safe extention of the
Swing `PropertyChangeEvent` system but BioJava interfaces explicitly
publish what types of event they may fire.

ChangeListener: The contract for handling events
------------------------------------------------

Objects that wish to be informed of change events must implement the
`ChangeListener` interface. This has just two methods:

-   `preChange(ChangeEvent ce)`
-   `postChange(ChangeEvent ce)`

An object will invoke `preChange` to inform listeners that it wishes to
alter its state. A `ChangeListener` may fire a `ChangeVetoException` to
prevent this change from taking place. The event source must respect
this. Once the event source has finished updating its state, it will
invoke the `postChangeEvent` method with an equivalent `ChangeEvent`
(one with the same values for its properties). The `postChange` method
should then take appropriate action to update the state of the listening
object.

There are two `ChangeListener` implementations supplied by default.
`ChangeListener.ALWAYS_VETO` always throws a `ChangeException` in
`preChange`. This object is useful if you wish to unconditionally lock
an object's property. In the exceptional circumstance when
`ChangeListener.ALWAYS_VETO` is registered and a `postChange` is
reached, it throws a `NestedError` with an assertion failure message.
This should only be able to happen if the event source is incorrectly
implemented.

`ChangeException.LOG_TO_OUT` prints all changes out to `System.out`. If
you want to log to a different stream, construct a new instance of
`ChangeListener.LoggingListener` with the stream.

Using ChangeSupport to implement Changeable
-------------------------------------------

To flag that an object is a source of change events, it should implement
`Changeable`. This interface has the following methods:

-   `addChangeListener(ChangeListener cl)`
-   `addChangeListener(ChangeListener cl, ChangeType ct)`
-   `removeChangeListener(ChangeListener cl)`
-   `removeChangeListener(ChangeListener cl, ChangeType ct)`

The methods with `ChangeType` arguments register the listener for that
type of event only. The methods without register the listener for all
events. Wherever possible, the type of event should be specified. This
potentialy allows for lazy instantiation of various resources and will
result in fewer events actualy being fired.

`ChangeSupport` is a utility class that handles 99% of the cases where
you wish to implement the `Changeable` interface. Idealy, you should
instantiate one of these objects and then delegate the listener methods
to this. In addition to the methods in `Changeable`, `ChangeSupport`
supplys the methods:

-   `firePreChangeEvent(ChangeEvent ce)`
-   `firePostChangeEvent(ChangeEvent ce)`

These methods invoke the `preChange` and `postChange` methods of the
apropreate listeners. `firePreChangeEvent` will pass on any
`ChangeVetoExceptions` that the listeners throw.

`AbstractChangeable` is an abstract implementation of `Changeable` that
delegates to a `ChangeSupport`. In the cases where your class does not
have to inherit from any class but must implement `Changeable`, this is
a perfect base class. It will lazily instantiate the delegate only when
listeners need to be registered.

In the [next
tutorial](BioJava:Tutorial:ChangeEvent_example_using_Distribution_objects "wikilink"),
we will implement an event source and add some listeners to it.

<Category:Tutorial>
