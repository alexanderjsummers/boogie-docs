The Boogie IVL language reference
*********************************

.. note::
  Some more information can be found in `This is Boogie 2 <http://research.microsoft.com/en-us/um/people/leino/papers/krml178.pdf>`_ .
  However this reference manual is **very** out-of-date and so doesn't accurately reflect the Boogie IVL as it exists today.
  None the less some may still find it to be a useful reference.

Types
=====

The following built-in types are avaible.

* ``bool`` - Boolean type
* ``int`` - Mathematical integer type
* ``Real`` - Mathematical real type

Bitvector types
---------------

A family of bitvector types of the form ``bv<N>`` are built-in
where ``<N>`` is the width of the bitvector. For example
``bv1`` and ``bv32`` are the bitvector types of a width 1 and width 32 bitvector respectively.

Note that bitvector types **are not signed**. The "signed-ness" is decided by operators, not
operands.

Map types
---------

Map types map a key of type ``K`` to a value of type ``V``.
Reading from locations not assigned to will return an unknown
value but the same value will returned for every read.

.. todo:: 
  Discuss map extensionality. Symbooglix assumes this property holds and Boogie
  does if you use the ``/useArrayTheory`` option but the original documentation
  states it doesn't hold and in Boogie's default mode it doesn't hold.

Examples

.. code-block:: boogie

  // Variable x maps integers to boolean values
  var x:[int]bool;
  // Variable y maps boolean values to boolean values
  var y:[bool]bool;

Maps may also be nested.

Example

.. code-block:: boogie

  // Variable a is a nested map that maps
  // integers to a map that maps 32-bit wide bitvectors
  // to booleans.
  var a:[int][bv32]bool;

Note a read like ``a[0]`` returns a map of type
``[bv32]bool``.

Multiple arity maps are also supported.

Example

.. code-block:: boogie

  // Variable c is 2-ary map that maps an ordered pair
  // for integers to booleans.
  var c:[int,int]:bool;

These are not the same as nested maps.

.. warning::
  Multiple arity maps may be less well supported by the various Boogie IVL
  backends because nested maps are generally preferred. Arguably Boogie IVL
  should not have two ways of basically doing this same thing. You are advised
  to avoid using multiple arity maps.

Creating new types
------------------

Additional types can be declared using :ref:`type_aliases` and
:ref:`type_constructors`.

Global declarations
====================

A Boogie IVL program consists of global declarations. The order of these
declarations does not matter.

Axioms
------

Semantics:

Axioms declare an expression that should be assumed to be true for the entire
lifetime of the program. A consequence of this is that axioms cannot refer to
**mutable** global variables.

.. warning::
  It is possible to declare axioms that individually or collectively are not
  satisfiable. This is a case of :ref:`inconsistent_assumptions`.


Grammar:

.. productionlist::
  axiom_stmt: "axiom" [ `attributes` ] `expr`;

Examples:

.. code-block:: boogie

  var a:int;
  var b:int;
  var map:[int]int;
  axiom {:some_attribute} a > b;
  axiom (forall x:int :: map[x] > a);

  const x:int;
  axiom x == 0;

Axioms may not refer to mutable global variables

.. code-block:: boogie

  var x:int;
  axiom x == 0; // ILLEGAL!


Functions
---------

Semantics:

.. todo:: Define semantics

Grammar:

.. todo:: Define grammar

Examples:

.. todo:: Give examples

Global Variables
----------------

Semantics:

Global variable declarations declare variables with an identifier ``Id`` at the
global program scope.  They can be mutable (``var``) or immutable (``const``).

Immutable variables can optionally have a ``unique`` qualifier. This qualifier
adds the assumption (i.e. like a axiom) that this variable has a different
value to all other global immutable variables of the same type ``Ty`` that have
the ``unique`` qualifier.

.. warning::
  It is possible to declare several global immutable variables to be unique and
  have axioms that state they have the same value. This is a case of
  :ref:`inconsistent_assumptions`.

.. todo:: Discuss order specifier and add to grammar

Grammar:

.. productionlist::
  global_var_decl: "var" [ `attributes` ] `Id`":"`Ty`;
  global_const_decl: "const" [ "unique" ] [ `attributes` ] `Id`":"`Ty`;

Examples:

.. code-block:: boogie

  var x:int; // Mutable global variable with identifier x
  var {:something} y:bool; // Mutable global variable with identifier y
  // Immutable global variable with idenitifer z.
  // Properties on this variable should be set using axiom(s)
  const z:bool;

Implementations
---------------

Semantics:

.. todo:: Define semantics

Grammar:

.. todo:: Define grammar

Examples:

.. todo:: Give examples

Procedures
----------

Semantics:

.. todo:: Define semantics

Grammar:

.. todo:: Define grammar

Examples:

.. todo:: Give examples

.. _type_aliases:

Type aliases
------------

Semantics:

.. todo:: Define semantics

Grammar:

.. todo:: Define grammar

Examples:

.. todo:: Give examples

.. _type_constructors:

Type constructors
-----------------

Semantics:

.. todo:: Define semantics

Grammar:

.. todo:: Define grammar

Examples:

.. todo:: Give examples

Expressions
===========

Boolean Constants
-----------------

``true``
^^^^^^^^

Semantics:

The constant that represents true.

``false``
^^^^^^^^^

Semantics:

The constant that represents false.

Boolean operators
-----------------

Logical and
^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Logical or
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Logical iff
^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Logical implication
^^^^^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Logical not
^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

For all
^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Exists
^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Bitvector constants
-------------------

Bitvector constants are written in the form ``<X>bv<N>`` where <X> is a positive decimal
integer which the bitvector represents and ``<N>`` is the width of the bitvector. ``<X>``
must be representable in a bitvector of width ``<N>``. Note that bitvectors are not signed.

Examples:

.. code-block:: boogie

  var x:bv8;

  x := 0bv8;  // 0b00000000
  x := 1bv8;  // 0b00000001
  x := 2bv8;  // 0b00000010
  x := 3bv8;  // 0b00000011
  x := 15bv8; // 0b11111111

Bitvector operators
-------------------

Concatenation
^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Extraction
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Other operators
^^^^^^^^^^^^^^^

No additional operators are defined. Additional functions (e.g. addition) can be used by
using a function declared with a ``:bvbuiltin`` string attribute where that attribute gives
the name of a bitvector function in the `SMT-LIBv2 QF_BV theory <http://smtlib.cs.uiowa.edu/logics-all.shtml#QF_BV>`_. The semantics of that function match the semantics of the bitvector function
named in the ``:bvbuiltin`` attribute.

Example:

.. code-block:: boogie

  // Arithmetic
  function {:bvbuiltin "bvadd"} bv8add(bv8,bv8) returns(bv8);
  procedure main()
  {
    var x:bv8;
  
    assert bv8add(1bv8, 1bv8) == 2bv8;
  }

Example declarations for ``bv8``:

Here is a **mostly** complete list of example function declarations for ``bv8``.
Similar declarations can be written for other bitvector widths. The names of the functions
are a suggestion only, any name can be used provided it does not conflict with other declarations.

.. code-block:: boogie

  // Arithmetic
  function {:bvbuiltin "bvadd"} bv8add(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvsub"} bv8sub(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvmul"} bv8mul(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvudiv"} bv8udiv(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvurem"} bv8urem(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvsdiv"} bv8sdiv(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvsrem"} bv8srem(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvsmod"} bv8smod(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvneg"} bv8neg(bv8) returns(bv8);

  // Bitwise operations
  function {:bvbuiltin "bvand"} bv8and(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvor"} bv8or(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvnot"} bv8not(bv8) returns(bv8);
  function {:bvbuiltin "bvxor"} bv8xor(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvnand"} bv8nand(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvnor"} bv8nor(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvxnor"} bv8xnor(bv8,bv8) returns(bv8);

  // Bit shifting
  function {:bvbuiltin "bvshl"} bv8shl(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvlshr"} bv8lshr(bv8,bv8) returns(bv8);
  function {:bvbuiltin "bvashr"} bv8ashr(bv8,bv8) returns(bv8);

  // Unsigned comparison
  function {:bvbuiltin "bvult"} bv8ult(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvule"} bv8ule(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvugt"} bv8ugt(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvuge"} bv8uge(bv8,bv8) returns(bool);

  // Signed comparison
  function {:bvbuiltin "bvslt"} bv8slt(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvsle"} bv8sle(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvsgt"} bv8sgt(bv8,bv8) returns(bool);
  function {:bvbuiltin "bvsge"} bv8sge(bv8,bv8) returns(bool);

Integer constants
-----------------

.. todo:: discuss integer constants

Integer operators
-----------------

Addition
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Subtraction
^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Negation
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Multiplication
^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Integer Division
^^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Real Division
^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Modulus
^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Coerce to real
^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Real constants
--------------

.. todo:: discuss real constants. Do we do rounding in boogie's parser here?

Real operators
--------------

Addition
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Subtraction
^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Negation
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Multiplication
^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Division
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Power
^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Coerce to Integer
^^^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Other overloaded operators
--------------------------

Some overloaded operators (e.g. ``+``) have already been discussed. Here are the other
overloaded operators.

Equality expressions
^^^^^^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

If then else expressions
^^^^^^^^^^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Old expressions
^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Unstructured implementations
============================

.. todo:: Discuss unstructured implementation structure, i.e. var decls at beginning then blocks


Commands
--------

Assignment
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

``assume``
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


``assert``
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

``call``
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


``call forall``
^^^^^^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


``goto``
^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


``havoc``
^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

``return``
^^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


Structured implementations
==========================

.. todo:: Discuss structure

Commands
--------

``break``
^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

``if``
^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples


``while``
^^^^^^^^^

Semantics:

.. todo:: Define semantics

Grammar

.. todo:: Define grammar

Examples

.. todo:: Give examples

Attributes
==========

.. todo:: Discuss attributes

Comments
========

A line that starts (skipping all non white space characters) with a ``//`` is treated as a comment line and the contents of that line should be ignored.

Example:

.. code-block:: boogie

  // This is a comment

Triggers
========

.. todo:: Discuss triggers

.. _inconsistent_assumptions:

Inconsistent assumptions
========================

When verifying an implementation a verifier should assume all of the following are true:

* All axioms declared
* All conditions declared in the implementation's ``requires`` clause
* The uniqueness of all global immutable variables declared with the ``unique`` qualifier

If these conditions are not satisfiable then the Boogie IVL program is said to have **inconsistent
assumptions** with respect to entry at that implementation.

If a Boogie IVL program has inconsistent assumptions it should be treated as correct, i.e.
the program is "vacuously correct".

If you wish to check a Boogie program for inconsistent assumptions there are several methods
for doing so

* Replace the implementation body with ``assert false``. If the program can be
  verified then (modulo bugs in the verifier) it must contain inconsistent
  assumptions. The :ref:`symbooglix_backend` backend has a program
  transformation pass that does the transformation described above that can be
  used separately from the main :ref:`symbooglix_backend` tool.

* Check the assumptions using :ref:`symbooglix_backend`.
  :ref:`symbooglix_backend` has a mode that will check assumptions before
  executing the Boogie IVL program.


Debug information
=================

.. todo:: Discuss how to represent debug information


Namespaces
==========

.. todo:: Discuss the different namespaces


Grammar
=======

.. todo:: Show entire grammar
