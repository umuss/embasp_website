=====================
Python implementation
=====================

The following figure provides some details about classes and interfaces of the implementation.

.. raw:: html

   <style>
   * {box-sizing: border-box;}

   .img-magnifier-container {
     position:relative;
   }

   .img-magnifier-glass {
     position: absolute;
     border: 3px solid #000;
     border-radius: 50%;
     cursor: none;
     /*Set the size of the magnifier glass:*/
     width: 220px;
     height: 220px;
   }
   </style>

   <script>
   function magnify(imgID, zoom) {
     var img, glass, w, h, bw;
     img = document.getElementById(imgID);
     /*create magnifier glass:*/
     glass = document.createElement("DIV");
     glass.setAttribute("class", "img-magnifier-glass");
     /*insert magnifier glass:*/
     img.parentElement.insertBefore(glass, img);
     /*set background properties for the magnifier glass:*/
     glass.style.backgroundImage = "url('" + img.src + "')";
     glass.style.backgroundRepeat = "no-repeat";
     glass.style.backgroundSize = (img.width * zoom) + "px " + (img.height * zoom) + "px";
     bw = 3;
     w = glass.offsetWidth / 2;
     h = glass.offsetHeight / 2;
     /*execute a function when someone moves the magnifier glass over the image:*/
     glass.addEventListener("mousemove", moveMagnifier);
     img.addEventListener("mousemove", moveMagnifier);
     /*and also for touch screens:*/
     glass.addEventListener("touchmove", moveMagnifier);
     img.addEventListener("touchmove", moveMagnifier);
     function moveMagnifier(e) {
       var pos, x, y;
       /*prevent any other actions that may occur when moving over the image*/
       e.preventDefault();
       /*get the cursor's x and y positions:*/
       pos = getCursorPos(e);
       x = pos.x;
       y = pos.y;
       /*prevent the magnifier glass from being positioned outside the image:*/
       if (x > img.width - (w / zoom)) {x = img.width - (w / zoom);}
       if (x < w / zoom) {x = w / zoom;}
       if (y > img.height - (h / zoom)) {y = img.height - (h / zoom);}
       if (y < h / zoom) {y = h / zoom;}
       /*set the position of the magnifier glass:*/
       glass.style.left = (x - w) + "px";
       glass.style.top = (y - h) + "px";
       /*display what the magnifier glass "sees":*/
       glass.style.backgroundPosition = "-" + ((x * zoom) - w + bw) + "px -" + ((y * zoom) - h + bw) + "px";
     }
     function getCursorPos(e) {
       var a, x = 0, y = 0;
       e = e || window.event;
       /*get the x and y positions of the image:*/
       a = img.getBoundingClientRect();
       /*calculate the cursor's x and y coordinates, relative to the image:*/
       x = e.pageX - a.left;
       y = e.pageY - a.top;
       /*consider any page scrolling:*/
       x = x - window.pageXOffset;
       y = y - window.pageYOffset;
       return {x : x, y : y};
     }
   }
   </script>

   <div class="img-magnifier-container">
     <img id="myimage" src="../_static/class_diagram_python.svg" width="1000" height="500">
   </div>

   <script>
   /* Initiate Magnify Function
   with the id of the image, and the strength of the magnifier glass:*/
   magnify("myimage", 2);
   </script>

Base module implementation
==========================

Each component in the :guilabel:`Base` module has been implemented by means of generic class or interface that will specialize in the following packages.

In particular, the |Handler|_ class collects |InputProgram|_ and |OptionDescriptor|_ objects communicated by the user.

For what the asynchronous mode is concerned, the class |Service|_ depends from the interface |Callback|_, since once the reasoning service has terminated, the result of the computation is returned back via a class |Callback|_.

Platforms module implementation
===============================

In order to support a new platform, the |Handler|_ and |Service|_ components must be adapted.

For the desktop platform we developed a |DesktopHandler|_ and a |DesktopService|_, which generalizes the usage of a solver on the desktop platform, allowing both synchronous and asynchronous execution modes.

Languages module implementation
===============================

This module includes specific classes for the management of input and output to ASP, Datalog and PDDL solvers.

The |Mapper|_ component of the :guilabel:`Languages` module is implemented via a |Mapper|_ class, that allows to translate input and output into Python objects.
Such translations are guided by `ANTLR4 <https://www.antlr.org/>`_ library and |Predicate|_ abstract class, also present in the module.

To make possible translate facts into strings and vice versa, the classes that want to represent a predicate, must extend the abstract class |Predicate|_, and must be implemented by including the following code:

* *predicateName="string_name"* : must be entered as a class field and must contain the predicate name (in the ASP/Datalog case) or the action name (in the PDDL case) to map;
* *[("class_field_name_1", int), ("class_field_name_2"), ...]* : Is a list that must be passed to super in the constructor, and must contain so many tuples how many are the class field, containing the field name, sorted by the position of the terms they represent, and optionally the keyword int if the field represents an integer.

Thanks to the structure of the |Predicate|_ class, this information is passed to the |Mapper|_ class, to correctly perform the translation mechanism.

If the classes intended for the translation are not constructed correctly in this way, an exception is raised.

In addition to the |Mapper|_, this module features three sub-modules which are more strictly related to ASP, Datalog and PDDL.

Specialization module implementation
====================================

The classes |DLVAnswerSets|_, |DLV2AnswerSets|_, |ClingoAnswerSets|_, |DLVHEXAnswerSets|_ implement specific extensions of the |AnswerSets|_ class, the |SPDPlan|_ class extends |Plan|_, while |IDLVMinimalModels|_ extends |MinimalModels|_ . These classes are in charge of manipulating the output of the respective solvers (e.g. IDLV).

Moreover, this module can contain classes extending |OptionDescriptor|_ to implement specific options of the solver at hand.

Class Diagram
=============

A complete UML Class Diagram is available `here <../_static/complete_diagram_python.svg>`_.

|

For further information, contact `embasp@mat.unical.it <embasp@mat.unical.it>`_ or visit our `website <https://www.mat.unical.it/calimeri/projects/embasp/>`_.

.. |Handler| replace:: ``Handler``
.. |InputProgram| replace:: ``InputProgram``
.. |OptionDescriptor| replace:: ``OptionDescriptor``
.. |Service| replace:: ``Service``
.. |CallBack| replace:: ``CallBack``
.. |DesktopHandler| replace:: ``DesktopHandler``
.. |DesktopService| replace:: ``DesktopService``
.. |Mapper| replace:: ``Mapper``
.. |Predicate| replace:: ``Predicate``
.. |DLVAnswerSets| replace:: ``DLVAnswerSets``
.. |DLV2AnswerSets| replace:: ``DLV2AnswerSets``
.. |ClingoAnswerSets| replace:: ``ClingoAnswerSets``
.. |DLVHEXAnswerSets| replace:: ``DLVHEXAnswerSets``
.. |SPDPlan| replace:: ``SPDPlan``
.. |AnswerSets| replace:: ``AnswerSets``
.. |Plan| replace:: ``Plan``
.. |IDLVMinimalModels| replace:: ``IDLVMinimalModels``
.. |MinimalModels| replace:: ``MinimalModels``

.. _Handler: ../_static/doxygen/python/classbase_1_1handler_1_1Handler.html
.. _InputProgram: ../_static/doxygen/python/classbase_1_1input__program_1_1InputProgram.html
.. _OptionDescriptor: ../_static/doxygen/python/classbase_1_1option__descriptor_1_1OptionDescriptor.html
.. _Service: ../_static/doxygen/python/classbase_1_1service_1_1Service.html
.. _CallBack: ../_static/doxygen/python/classbase_1_1callback_1_1Callback.html
.. _DesktopHandler: ../_static/doxygen/python/classplatforms_1_1desktop_1_1desktop__handler_1_1DesktopHandler.html
.. _DesktopService: ../_static/doxygen/python/classplatforms_1_1desktop_1_1desktop__service_1_1DesktopService.html
.. _Mapper: ../_static/doxygen/python/classlanguages_1_1mapper_1_1Mapper.html
.. _Predicate: ../_static/doxygen/python/classlanguages_1_1predicate_1_1Predicate.html
.. _DLVAnswerSets: ../_static/doxygen/python/classspecializations_1_1dlv_1_1dlv__answer__sets_1_1DLVAnswerSets.html
.. _DLV2AnswerSets: ../_static/doxygen/python/classspecializations_1_1dlv2_1_1dlv2__answer__sets_1_1DLV2AnswerSets.html
.. _ClingoAnswerSets: ../_static/doxygen/python/classspecializations_1_1clingo_1_1clingo__answer__sets_1_1ClingoAnswerSets.html
.. _DLVHEXAnswerSets: ../_static/doxygen/python/classspecializations_1_1dlvhex_1_1dlvhex__answer__sets_1_1DLVHEXAnswerSets.html
.. _IDLVMinimalModels: ../_static/doxygen/python/classspecializations_1_1idlv_1_1idlv__minimal__models_1_1IDLVMinimalModels.html
.. _SPDPlan: ../_static/doxygen/python/classspecializations_1_1solver__planning__domains_1_1spd__plan_1_1SPDPlan.html
.. _AnswerSets: ../_static/doxygen/python/classlanguages_1_1asp_1_1answer__sets_1_1AnswerSets.html
.. _MinimalModels: ../_static/doxygen/python/classlanguages_1_1asp_1_1minimal__models_1_1MinimalModels.html
.. _Plan: ../_static/doxygen/python/classlanguages_1_1pddl_1_1plan_1_1Plan.html


