.. image:: _image/EmbASP_Logo.svg
   :height: 300 px
   :width: 500 px
   :align: center

======
EmbASP
======

A framework for the integration (embedding) of **Logic Programming** in external systems for generic applications.
It helps developers at designing and implementing complex reasoning tasks by means of solvers on different platforms.

The framework can be implemented in a object-oriented programming language of choice, easing and guiding the generation of suitable libraries for the use of specific solvers on selected platforms.
We currently provide 3 implementations (in `Java <https://www.java.com>`_ , in `Python <https://www.python.org>`_ and in `C# <https://docs.microsoft.com/en-us/dotnet/csharp/>`_ ) 
and ready-made libraries for the embedding several logic programming languages (mainly on the Desktop platform). In particular, we provide support for: 

- ASP (Answer Set Programming) solvers

   - `DLV <http://www.dlvsystem.com/dlv>`_ (also for the Android platform, for the Java language)
   - `DLV2 <https://www.mat.unical.it/DLV2>`_ 
   - `clingo <https://potassco.org/clingo>`_ 
   - `DLVHEX <http://www.kr.tuwien.ac.at/research/systems/dlvhex/>`_ 

- PDDL (Planning Domain Definition Language) 

   - Cloud solver `Solver.Planning.Domains <http://solver.planning.domains>`_ (also for the Android platform, for the Java language).

- Datalog

   - `I-DLV <https://github.com/DeMaCS-UNICAL/I-DLV>`_ 

However, the framework has been designed to be easily extensible and adaptable to different solvers and platforms.
It is worth to notice that solvers are invoked in different modes; for instance, SPD is invoked via a remote connection, while for the other, binaries are effectively embedded and natively executed.


Documentation
=============

.. toctree::
   :caption: Documentation
   :hidden:

   documentation/java-implementation.rst
   documentation/python-implementation.rst
   documentation/csharp-implementation.rst
   documentation/technical_doc.rst

Implementations
---------------

*  :doc:`documentation/java-implementation`
*  :doc:`documentation/python-implementation`
*  :doc:`documentation/csharp-implementation`

Technical documentation
-----------------------

*  :doc:`documentation/technical_doc`

Examples
========

.. toctree::
   :caption: Examples
   :hidden:

   examples/shortest-path-asp-java.rst
   examples/shortest-path-asp-python.rst
   examples/shortest-path-asp-csharp.rst
   examples/blocks-world-pddl-java.rst
   examples/blocks-world-pddl-python.rst
   examples/blocks-world-pddl-csharp.rst
   examples/transitive-closure-datalog-java.rst
   examples/transitive-closure-datalog-python.rst
   examples/transitive-closure-datalog-csharp.rst
   examples/sudoku-android.rst

Desktop ASP exemples
--------------------

* :doc:`examples/shortest-path-asp-java`
* :doc:`examples/shortest-path-asp-python`
* :doc:`examples/shortest-path-asp-csharp`

Desktop PDDL examples
---------------------

*  :doc:`examples/blocks-world-pddl-java`
*  :doc:`examples/blocks-world-pddl-python`
*  :doc:`examples/blocks-world-pddl-csharp`

Desktop Datalog examples
------------------------

*  :doc:`examples/transitive-closure-datalog-java`
*  :doc:`examples/transitive-closure-datalog-python`
*  :doc:`examples/transitive-closure-datalog-csharp`

Android example
---------------

*  :doc:`examples/sudoku-android`

Contacts
========

For further information, contact `embasp@mat.unical.it <embasp@mat.unical.it>`_ or visit our `website <https://www.mat.unical.it/calimeri/projects/embasp/>`_.


