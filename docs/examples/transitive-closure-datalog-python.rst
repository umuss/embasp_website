=================================
Transitive Closure Datalog Python
=================================

Getting started
===============

The framework is released as WHL file to be used on a Desktop platform, therefore it can be easily installed via pip.

The framework needs `ANTLR4 <https://www.antlr.org>`_ library for its operation. 

Using EmbASP
============

In the following, we describe an actual usage of the framework by means of a running example;
as a use case, we will develop a simple Desktop application to compute the transitive closure of a graph.

The complete code of this example is freely available `here <https://github.com/DeMaCS-UNICAL/EmbASP-Python/tree/master/test/specialization/idlv>`_.

.. image:: ../_image/transitive_closure.png
   :align: center

We will make use of the annotation-guided mapping, in order to create Python object constituting Datalog predicates.

To this purpose, the following classes are intended to represent possible predicates that a Datalog program can use:

.. code-block:: python

  class Edge(Predicate):
      predicate_name = "edge"

      def __init__(self, source=None, destination=None):
          Predicate.__init__(self, [("source"),("destination")])
          self.source = source
          self.destination = destination

      [...]
  

.. code-block:: python

  class Path(Predicate):
      predicate_name = "path"

      def __init__(self, source=None, destination=None, weight=None):
          Predicate.__init__(self, [("source"),("destination")])
          self.source = source
          self.destination = destination

      [...]
  

At this point, supposing that we have embedded the IDLV Datalog engine in this project, we can start deploying our application:

.. code-block:: python

  def test_find_reachable_nodes(self):
    try:
        input = DatalogInputProgram()
        handler = DesktopHandler(IDLVDesktopService("executable/idlv"))
        DatalogMapper.get_instance().register_class(Path)
        input.add_program("path(X,Y) :- edge(X,Y).")
        input.add_program("path(X,Y) :- path(X,Z), path(Z,Y). ")
        handler.add_program(input)

        input.add_object_input(Edge(1,2))
        input.add_object_input(Edge(2,3))
        input.add_object_input(Edge(2,4))
        input.add_object_input(Edge(3,5))
        input.add_object_input(Edge(5,6))

        minimalModels = handler.start_sync()

        for o in minimalModels.get_minimal_models().pop().get_atoms_as_objectset():
            if isinstance(o, Path):
                print(o.__str__())

    except Exception as e:
        print(str(e))



The main method contains an |Handler|_ instance, that is initialized with a |DesktopHandler|_ using the parameter |IDLVDesktopService|_ with a string representing the path to the IDLV local grounder.

The |DatalogMapper|_ registers the classes created before in order to manage the input and output objects.

A string and a list of ``Edge`` objects representing facts, rules and constraints of the Datalog program are added to an |DatalogInputProgram|_, and the |DatalogInputProgram|_ is added to the |Handler|_.

Finally the solver is invoked, and the output is retrieved.

In this example the ``Path`` predicates, represent all the arcs in the transitive closure of the starting graph. The output predicates can be managed accordingly to the user's desiderata, as they are simply objects. 


For further information, contact `embasp@mat.unical.it <embasp@mat.unical.it>`_ or visit our `website <https://www.mat.unical.it/calimeri/projects/embasp/>`_.

.. |Handler| replace:: ``Handler``
.. |DesktopHandler| replace:: ``DesktopHandler``
.. |IDLVDesktopService| replace:: ``IDLVDesktopService``
.. |DatalogMapper| replace:: ``DatalogMapper``
.. |DatalogInputProgram| replace:: ``DatalogInputProgram``

.. _Handler: ../_static/doxygen/python/classbase_1_1handler_1_1Handler.html
.. _DesktopHandler: ../_static/doxygen/python/classplatforms_1_1desktop_1_1desktop__handler_1_1DesktopHandler.html
.. _IDLVDesktopService: ../_static/doxygen/python/classspecializations_1_1idlv_1_1desktop_1_1idlv__desktop__service_1_1IDLVDesktopService.html
.. _DatalogMapper: ../_static/doxygen/python/classlanguages_1_1datalog_1_1datalog__mapper_1_1DatalogMapper.html
.. _DatalogInputProgram: ../_static/doxygen/python/classlanguages_1_1datalog_1_1datalog__input__program_1_1DatalogInputProgram.html

