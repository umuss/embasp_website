====================
Blocks-world PDDL C#
====================

Getting started
===============

The framework is released as DLL file to be used on a Desktop platform, therefore it can be easily added and used in any C# project.

Using EmbASP
============

In the following, we describe an actual usage of the framework by means of a running example;
as a use case, we will develop a simple Desktop application to solve the blocks-world problem.

The complete code of this example is freely available `here <https://www.mat.unical.it/calimeri/projects/embasp/files/EmbASP_Desktop_BlocksWorld_CSharp.zip>`_.

.. image:: ../_image/blocks-world.svg
   :align: center

We will make use of the annotation-guided mapping, in order to retrieve the actions constituting a PDDL plan via C# objects.

To this purpose, the following classes are intended to represent possible actions that a blocks-world solution plan can feature:

.. code-block:: csharp

  [Id("pick-up")]
    class PickUp
    {
    	[Param(0)]
    	private string block;
    	
        [...]
    }

.. code-block:: csharp

  [Id("put-down")]
    class PutDown
    {
    	[Param(0)]
    	private string block;
    	
    	[...]
    }

.. code-block:: csharp

  [Id("stack")]
    class Stack
    {
    	[Param(0)]
    	private string block1;
    	
    	[Param(1)]
    	private string block2;
    	
    	[...]
    }

.. code-block:: csharp

  [Id("unstack")]
    class Unstack
    {
    	[Param(0)]
    	private string block1;
    	
    	[Param(1)]
    	private string block2;
    	
    	[...]
    }
            

At this point, supposing that we are given two files defining the blocks-world domain and a problem instance, we can start deploying our application:

.. code-block:: csharp

  class Program
  {
      static void Main(string[] args)
      {
          Handler handler = new DesktopHandler(new SPDDesktopService());

          InputProgram inputDomain = new PDDLInputProgram(PDDLProgramType.DOMAIN);
          inputDomain.AddFilesPath("domain.pddl");

          InputProgram inputProblem = new PDDLInputProgram(PDDLProgramType.PROBLEM);
          inputProblem.AddFilesPath("p01.pddl");

          handler.AddProgram(inputDomain);
          handler.AddProgram(inputProblem);

          try
          {
              PDDLMapper.Instance.RegisterClass(typeof(PickUp));
              PDDLMapper.Instance.RegisterClass(typeof(PutDown));
              PDDLMapper.Instance.RegisterClass(typeof(Stack));
              PDDLMapper.Instance.RegisterClass(typeof(Unstack));

              Plan plan = (Plan)handler.StartSync();

              foreach(object obj in plan.ActionsObjects)
              {
                  if (typeof(PickUp).IsInstanceOfType(obj) || typeof(PutDown).IsInstanceOfType(obj) ||
                        typeof(Stack).IsInstanceOfType(obj) || typeof(Unstack).IsInstanceOfType(obj)) 
                  {
                     Console.WriteLine(obj.ToString());
                  }
              }

          }
          catch (Exception e)
          { 
              Console.WriteLine(e.Message);
          }
      }
  }


The class contains an |Handler|_ instance as field, that is initialized with a |DesktopHandler|_ using the required parameter |SPDDesktopService|_.

Then it's set-up the input to the solver; since PDDL requires separate definitions for domain and problem, two |PDDLInputProgram|_ are created and then given to the handler.

The next lines inform the |PDDLMapper|_ about what classes are intended to map the output actions.

Finally the solver is invoked, and the output is retrieved.

The output actions can be managed accordingly to the user's desiderata. 

|

For further information, contact `embasp@mat.unical.it <embasp@mat.unical.it>`_ or visit our `website <https://www.mat.unical.it/calimeri/projects/embasp/>`_.

.. |Handler| replace:: ``Handler``
.. |DesktopHandler| replace:: ``DesktopHandler``
.. |SPDDesktopService| replace:: ``SPDDesktopService``
.. |PDDLInputProgram| replace:: ``PDDLInputProgram``
.. |PDDLMapper| replace:: ``PDDLMapper``

.. _Handler: ../_static/doxygen/cSharp/classbase_1_1Handler.html
.. _DesktopHandler: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1platforms_1_1desktop_1_1DesktopHandler.html
.. _SPDDesktopService: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1specializations_1_1solver__planning__domains_1_1desktop_1_1SPDDesktopService.html
.. _PDDLInputProgram: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1languages_1_1pddl_1_1PDDLInputProgram.html
.. _PDDLMapper: ../_static/doxygen/cSharp/classit_1_1unical_1_1mat_1_1embasp_1_1languages_1_1pddl_1_1PDDLMapper.html

