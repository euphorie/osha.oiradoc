.. Tool change analysis documentation master file, created by
   sphinx-quickstart on Mon Jul  9 09:19:42 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

==============================
Tool change analysis for #3539
==============================

The purpose of this report was to analyse OiRA's behaviour whenever a published tool is changed and republished in
different ways.

.. note:: 
   In all the examples below, even if not explicitly mentioned, it is assumed that the tool is republished after any changes have been applied.


The Update Page
===============


Every time a tool is changed and republished, the client user is redirected to an update
page in the preparation section:

.. image:: images/tool_update_page.png

Risks
=====

---------------
Changing a Risk
---------------

When changing a risk, the values in the identification step are still kept. 
A risk that was identified as being present, will still be marked as present
(red block). The same applies to optional modules and profile questions.

In the evaluation section, for trivial changes the settings are also kept.
However, if a risk's type is changed to *Policfy* (from either *Risk* or *Priority Risk*), 
then the evaluation step associated with that risk is completely removed.

Changing the risk's type back to *Risk* or *Priority Risk* again doesn't
restore the old settings from the evaluation step.

.. image:: images/evaluation_settings_kept.png

Every singly time a risk is changed and the tool updated, all measures in 
the action plan step are removed (even if it was only
a trivial change like editing the risk's image caption or description).

.. image:: images/action_plan_measures_are_lost.png

-------------
Adding a Risk
-------------

When a new risk is added in the CMS, the other pre-existing risks' settings in the
identification and evaluation steps are kept and the only difference is that
the new risk is in the client.

In the action plan step however, *all measures from all risks are removed.*

.. note:: The newly added risk will only be visible in the evaluation step if the client user identifies it as being present in the identification step.

.. image:: images/risk_added_identification_step.png

---------------
Removing a Risk
---------------


When an existing risk is removed in the CMS, the other pre-existing risks' settings in the
identification and evaluation steps are kept and the particular risk simply
removed from the tool.

In the action plan step however, *all measures from all risks are removed.*

Measures
========

----------------------------------
Adding/Editing/Deleting a measure 
----------------------------------

When a measure is added/edited/deleted inside a risk:

* The identification settings for that risk are kept.
* The evaluation settings for that risk are kept.
* Any measures added in the action plan step are lost.

Modules
=======

--------------------------------
Adding/Editing/Deleting a module
--------------------------------

When a module is added/edited/deleted inside a tool:

* The identification settings for all existing risks are kept.
* The evaluation settings for all existing risks are kept.
* Any measures added in the action plan step are lost.


Profile Question
================

------------------------------------------
Adding/Editing/Deleting a profile question
------------------------------------------

As always, the user is directed to an update page. In this case, the new
profile question will be visible and the user will have to option to
enable/disable optional profile questions and add repeatable profile questions.

Whenever a profile question is added/edited/deleted inside a tool:

* The identification settings for all existing risks are kept.
* The evaluation settings for all existing risks are kept.
* Any measures added in the action plan step are lost.


.. image:: images/new_profile_questions.png


Summary
=======

All saved measures in the action plan step of a tool are removed every time
the tool is republished.

The settings in the identification and evaluation steps are however kept,
except if a risk is changed to a *Policy Risk* (then the evaluation step's
settings are removed since the risk should no longer be manually evaluated).

--------------------------------------
Detailed answers on questions in #3539
--------------------------------------

*EU-OSHA wrote:*

    *if a profile question/module/risk/measure is edited (in terms of content, but
    the item is kept) the related identification and evalutation phases in the
    client in the existing sessions should be kept as they are.* 

*Answer*

	*This is what currently happens.*

*EU-OSHA wrote:*
    
    *I'm not sure what happens in the action plan.*

*Answer*

	*Every time the tool is changed and republished, all action plan measures are removed.*

*EU-OSHA wrote:*

    *if a profile question/module/risk/measure is deleted, the related steps in
    the client (identification, evaluation, a.plan) should be deleted.*

*Answer*

	*This is what currently happens.*



