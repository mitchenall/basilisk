.. _collections:

===========
Collections
===========

Basilisk has good implementations of the most important data structures for 
application work.  All collections are **immutable** - methods which would
mutate them (in normal code) return new versions of the collections.

Where time complexity is stated, recall that log[32] of 1 billion is just 
less than 6.

Vector
======

.. class:: Vector
    
    The Vector class is a random access data structure which can be accessed
    by numeric key.  Push, pop and set are all O(log[32] n), which makes the
    time complexity very low for practical datasets.

    Note that - as for all Basilisk collections - the constructor is private
    and the ``from`` static method should be used instead. 

.. staticmethod:: Vector.from([source : mixed]) -> Vector

    Creates a new Vector from the specified source object.

    :param source: 
        An ``array``, ``Vector``, or object with a ``.forEach`` method.  This
        will be iterated to fill the vector.  Passing ``null`` will result in
        an empty Vector.

.. attribute:: Vector.length : number

    The number of elements in the Vector.  This is a pre-computed property,
    so access is O(1)

.. method:: Vector.get(index : number) -> any

    Retrieve the value at a particular position in the Vector.  Note that
    (unlike Javascript arrays) retrieving a position which is outside the
    range of the collection is an Error.
    Time complexity: O(log[32] n)

    :param index:
       The position in the vector to return.  Must be in the range
       (-length ; length).  Negative indexes are interpreted as being from
       the end of the vector (ie. ``.length + index``).

.. method:: Vector.push(value : any) -> Vector

    Creates a **new Vector** which has the specified value in its last 
    position.  The instance on which it is called is not modified.
    Time complexity: O(log[32] n)

.. method:: Vector.set(index : number, value : any) -> Vector

    Creates a **new Vector** which has the specified position replaced
    with the specified value.  If the value ```===``` the current value
    in that position, will return ``this``.
    Time complexity: O(log[32] n)

    :param index: a number in the range (-length ; length).  



.. method:: Vector.pop() -> Vector

    Returns a **new Vector** which has the item in the final position 
    removed.  
    Time complexity: O(log[32] n)

.. method:: Vector.peek() -> any
    
    Returns the last element in the Vector.

.. method:: Vector.forEach(callback: function (item : any, index : number), context:any)
    
    Iterates over the Vector in order, calling the ``callback`` for each 
    element in turn.  It is perfectly valid to pass a function which takes 
    fewer arguments (ie. ``function (item)`` instead of ``function (item, key)`` - 
    this is handled natively by Javascript). 

.. method:: Vector.equals(other : any) -> boolean

    Checks whether the two Vectors are **equal**.  Each element is checked in 
    turn.  If all elements are **equal** (see :ref:`equality-protocol`)

    :param other: 
        Another object to check for equality.  If this is **not** a Vector, this 
        will never return true.

StringMap
=========

.. class:: StringMap

    A ``HashMap`` of ``strings`` to any other object.  In Typescript, this class
    is generic on type ``T`` of the stored objects.

    Note that - as for all Basilisk collections - the constructor is private
    and the ``from`` static method should be used instead. 

.. staticmethod:: StringMap.from([source : mixed]) -> StringMap

    Create a new StringMap from the specified source object.  

    If the object is a StringMap, then that object is returned directly.  

    Finally, the object is iterated using ``for in`` and own properties
    are added to the map.

.. method:: StringMap.get(key : string[, default: any = undefined]) -> any

    x

.. method:: StringMap.set(key : string, value: any) -> StringMap

    x

.. method:: StringMap.delete(key : string) -> StringMap

    x

.. method:: StringMap.forEach(function (value : any, key : string) [, context: any = undefined]) -> any

    x

HashMap
=======

.. class:: HashMap

    A configurable HashMap of values.  In Typescript, this class
    is generic on type ``T`` of the stored objects, type ``K`` of keys.

    Note that - as for all Basilisk collections - the constructor is private
    and the ``from`` static method should be used instead. 

.. staticmethod:: HashMap.from(hashFn: function (key : any) -> Number, [source : mixed]) -> Vector

    Create a new HashMap from the specified source object.  The ``hashFn`` will
    be called every time the hash of a key needs to be evaluated, and should
    handle any object you might use as a key.  ``basilisk.hashCode`` is a 
    standard implementation which should handle most important cases.

    If the object is a HashMap and its hashFn ``===`` the provided function,
    then it will be returned directly.  Otherwise it will be iterated and
    each key passed through the provided hashFunction.

    Finally, the object is iterated using ``for in`` and own properties
    are added to the map.

.. method:: HashMap.get(key : any[, default: any = undefined]) -> any

    x

.. method:: HashMap.set(key : any, value: any) -> HashMap

    x

.. method:: HashMap.delete(key : any)

    x

.. method:: HashMap.forEach(function (value : any, key : any) [, context: any = undefined])

    x


.. function:: hashCode(key:any) -> uint

    Generate a hashCode for the provided object.  If the object has a 
    ``hashCode`` method, that will be called and the return returned.  
    For strings, numbers, booleans, null and undefined, a default hash 
    implementation is used.

    If none of the above apply a TypeError is thrown.

    Hash functions should be fast, deterministic, and well distributed over
    the integers.
