=============================
Transitive Closure Datalog C#
=============================

Getting started
===============

The framework is released as DLL file to be used on a Desktop platform, therefore it can be easily imported and used in any C# project.

The framework needs `ANTLR4 <https://www.antlr.org>`_ library for its operation. 

Using EmbASP
============

In the following, we describe an actual usage of the framework by means of a running example;
as a use case, we will develop a simple Desktop application to compute the transitive closure of a graph.

The complete code of this example is freely available `here <https://github.com/DeMaCS-UNICAL/EmbASP-CSharp/tree/master/it/unical/mat/test/specialization/idlv>`_.

.. image:: ../_image/transitive_closure.png
   :align: center

We will make use of the annotation-guided mapping, in order to create CSharp object constituting Datalog predicates.

To this purpose, the following classes are intended to represent possible predicates that a Datalog program can use:

.. code-block:: csharp

    [Id("path")]
    class Path
    {
        [Param(0)]
        private int from;

        [Param(1)]
        private int to;

        public Path()
        {
            this.from = 0;
            this.to = 0;
        }

        public Path(int from, int to)
        {
            this.from = from;
            this.to = to;
        }

        [...]
    }
    

.. code-block:: csharp

    [Id("edge")]
    class Edge
    {
        [Param(0)]
        private int from;

        [Param(1)]
        private int to;

        public Edge()
        {
            this.from = 0;
            this.to = 0;
        }

        public Edge(int from, int to)
        {
            this.from = from;
            this.to = to;
        }

        [...]
    }

At this point, supposing that we have embedded the IDLV Datalog engine in this project, we can start deploying our application:

.. code-block:: csharp

    public class TransitiveClosure
    {
        public static void Main(string[] args)
        {
            try
            {
                InputProgram input = new DatalogInputProgram();
                DesktopHandler handler = new DesktopHandler(new IDLVDesktopService("executables/idlv"));
                DatalogMapper.Instance.RegisterClass(typeof(Path));
                input.AddProgram("path(X,Y) :- edge(X,Y).");
                input.AddProgram("path(X,Y) :- path(X,Z), path(Z,Y).");
                handler.AddProgram(input);

                input.AddObjectInput(new Edge(1,2));
                input.AddObjectInput(new Edge(2,3));
                input.AddObjectInput(new Edge(2,4));
                input.AddObjectInput(new Edge(3,5));
                input.AddObjectInput(new Edge(3,6));    

                IDLVMinimalModels minimalModels = (IDLVMinimalModels)handler.StartSync();

                foreach (MinimalModel m in minimalModels.Minimalmodels)
                {
                    foreach (object a in m.Atoms)
                    {
                        if (typeof(Path).IsInstanceOfType(a))
                        {
                            Console.WriteLine(a);
                        }
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine(e.ToString());
            }
        }

  }

The main method contains an |Handler|_ instance, that is initialized with a |DesktopHandler|_ using the parameter |IDLVDesktopService|_ with a string representing the path to the IDLV local grounder.

The |DatalogMapper|_ registers the classes created before in order to manage the input and output objects.

A string and a list of ``Edge`` objects representing facts, rules and constraints of the Datalog program are added to an |DatalogInputProgram|_, and the |DatalogInputProgram|_ is added to the |Handler|_.

Finally the solver is invoked, and the output is retrieved.

In this example the ``Path`` predicates, represent all the arcs in the transitive closure of the starting graph. The output predicates can be managed accordingly to the user's desiderata, as they are simply Objects. 


For further information, contact `embasp@mat.unical.it <embasp@mat.unical.it>`_ or visit our `website <https://www.mat.unical.it/calimeri/projects/embasp/>`_.

.. |Handler| replace:: ``Handler``
.. |DesktopHandler| replace:: ``DesktopHandler``
.. |IDLVDesktopService| replace:: ``IDLVDesktopService``
.. |DatalogMapper| replace:: ``DatalogMapper``
.. |DatalogInputProgram| replace:: ``DatalogInputProgram``

.. _Handler: ../_static/doxygen/cSharp/classbase_1_1Handler.html
.. _DesktopHandler: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1platforms_1_1desktop_1_1DesktopHandler.html
.. _IDLVDesktopService: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1specializations_1_1idlv_1_1desktop_1_1IDLVDesktopService.html
.. _DatalogMapper: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1languages_1_1datalog_1_1DatalogMapper.html
.. _DatalogInputProgram: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1languages_1_1datalog_1_1DatalogInputProgram.html

