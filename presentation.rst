
.. title: Turning what you want into what you got, Behaviour Driven Design through Python

----

Turning what you want into what you got
=======================================


.. code:: Gherkin

    Feature: Python & BDD
        As a Python Developer
        I want to get work done
        easier, quicker and with no misunderstanding
        As this makes my life easier

Behaviour Driven Design through Python

----

About me
========

- Robotics Engineer
- With Python for 5 years
- Manufacturing, Web / Cloud hosting, Financial Services
- Currently manage a team of 8


(And ask questions at any point, this is a narrative not a lecture)

----

The Problem
===========

"I thought it should be"

"Wasn't in the BRD/Requirements/X"

"Oh, if only we'd thought of that"

"Outside the scope of this"

Generally ambiguous requirements

----

And my favourite
================

"Make it work like it does now, but different"


----

What we'd all like / want
=========================
"Instead of a business stakeholder passing requirements to the development team without much opportunity for feedback,
the developer and stakeholder collaborate to write automated tests that 
**express the outcome that the stakeholder wants**"

The Cucumber Book By: Matt Wynne; Aslak Hellesoy

----

How can we achieve this?
========================

* **Communicate**

* Work through examples

* Specification by example

----

What is BDD?
============

* Specify the *Desired* behaviour

* Do this for each *Feature*/story 

* In collaboration, the 3 amigos together [#f1]_

* Gherkin & Behave a pretty good match


.. [#f1] http://neilkillick.com/2013/10/11/revisiting-the-three-amigos/

----

And why?
========

* Clarity of Purpose

* Clear concise examples

* Living Documentation

* Greater understanding of the **Desired** functionality

----

Our problem
===========

.. code:: Gherkin

    Feature: Regattas
      As a Regatta organizer
      I want to be able to see a list of all Regattas
      and track their races and participants
      So that I can manage the even effectively

----

.. code:: Gherkin

  Scenario: View regattas
    Given I am on the Sail Race Scoring website
    When I view all Regattas
    Then I see there are 2 Regattas:
    | Regatta Name |
    | Nationals |
    | Europeans |

----

.. code:: Gherkin

  Background: Log on to the SRS Website
    Given I open a webbrowser
    and I log in to the Sail Race Scoring website

  Scenario: Add a new Regatta
    Given I am viewing all Regattas
    When I add a new Regatta with the name "My Regatta"
    Then I see there are 3 Regattas:
    | Regatta Name |
    | Nationals |
    | Europeans |
    | My Regatta |

----

Behave
======

Behave [#f2]_ is what I've found to be the 
best BDD library for Python

It works around

* Feature files
* Steps
* Context / World
* Environment
* Tags

.. [#f2] http://pythonhosted.org/behave/

----

Steps
=====

A python function decorated with ``@given``, ``@when`` or ``@then`` to
match to a step in the feature.

Parsed using ``parse``, ``cfparse`` or ``re`` so allows a lot of flexibility

Passes if it doesn't throw an exception

.. code:: python

    assert 'Success' in page_text

----

Context / World
===============

Captures the state of the current scenario, called ``context``
and passed to every Step.

What should go into this?

* Sessions
* Test helpers (selenium)
* Anything needed for the step

Automatically cleaned up after each step

----

Setup / Tear down
=================

* before_step, after_step
* before_scenario, after_scenario
* before_feature, after_feature
* before_tag, after_tag
* before_all, after_all


----

Behave applied
==============

----


Technical Tooling
=================

----


Demo?
=====

----

Any questions?
==============

----

Contact Me
==========

Twitter: @_davem
Github: githubcom/dave-m
Blog: david.mcilwee.me
