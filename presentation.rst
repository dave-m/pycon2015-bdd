
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

What this talk is about
=======================

The **why** much more than the how.

*(the hard bit to get right)*

----

.. image:: http://image.slidesharecdn.com/introductiontosoftwareengineering-100815051502-phpapp01/95/introduction-to-software-engineering-2-728.jpg?cb=1281851199

----

About me
========

- Robotics Engineer
- With Python for 5 years
- Manufacturing, Web / Cloud hosting, Financial Services
- Currently lead a multinational team of 8 across a global business
- Not great at CSS


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

What we'd all like
==================
"Instead of a business stakeholder passing requirements to the development team without much opportunity for feedback,
the developer and stakeholder collaborate to write automated tests that 
**express the outcome that the stakeholder wants**"

The Cucumber Book By: Matt Wynne; Aslak Hellesoy

----

How can we achieve this?
========================

**Talk together**

Work through examples

Define the Specification by examples

>> Behaviour Driven Design

----

What is BDD?
============

* Specify the *Desired* behaviour

* Do this for each *Feature*/story 

* In collaboration, the 3 amigos together [#f1]_

.. image:: http://neilkillick.com/wp-content/uploads/2013/10/three-amigos-300x194.png 

* Gherkin & Behave a pretty good match


.. [#f1] http://neilkillick.com/2013/10/11/revisiting-the-three-amigos/

----

And why?
========

* Clarity of Purpose

* Greater understanding of the **Desired** functionality

* Living Documentation full of examples

* Frictionless path to automation / Continuous Integration / Continuous Delivery

----

Engaging with the Stakeholder
=============================

Interested people are engaged people

Engaged people are passionate about the idea

Passionate people stick with the idea

BDD gives a lot of clarity and removes time & frustration

More time to concentrate on the important things (making $$$)


----


An example Feature
==================

.. image:: http://www.sail-world.com/photo/photos_2012_3/Alt_London201212cb_04475.jpg 

----

.. code:: gherkin

    Feature: Races
      As a Regatta organizer
      I want to be able to see a list of all Races
      and track their participants
      So that I can manage the event effectively

      Scenario: View all Races for a Regatta
        Given I am viewing the "Nationals" Regatta
        When I click on the "Races" button
        Then I see there are 2 Races:
          | Race Name   | Course    | Laps | Competitors | Completed |
          | First Race  | Triangle  | 3    | 0           | No        |
          | Second Race | Trapezoid | 2    | 0           | No        |


----

The technical bit
=================

(Behind the smoke and mirrors)

----

Behave
======

Behave [#f2]_ is what I've found to be the 
best BDD library for Python

It works around

* Environment
* Tags
* Context / World
* Feature files
* Steps


.. [#f2] http://pythonhosted.org/behave/

----

The `Tutorial <http://pythonhosted.org/behave/tutorial.html>`_ is very good 

----

Environment
===========

The overall Setup / Tear down

* before_step, after_step
* before_scenario, after_scenario
* before_feature, after_feature
* before_tag, after_tag
* before_all, after_all

.. code:: gherkin

    Feature: Something New
      As a somebody
      I want to do things
      As this is what I want to do

      @wip
      Scenario: Make it work
        Given I have something
        When I do something else
        Then it works

----

Tags
====

Categorize (tag) scenario's features

Usefull for

* Marking slow / Work in progress tests
* Tests that perform data manipulation
* Setting up certain technical things

.. code:: python

    def before_feature(context, feature):
        """Open a selenium browser if required"""
        if 'ui' in feature.tags:
            context.browser = webdriver.Chrome('/usr/lib/chromium/chromedriver')

*This enables you to have a safe sub-set of tests to run
against live data, or exclude known WIP Features*

----

State / Context / World
=======================

``context`` captures the state of the current scenario / step

.. code:: python

    def my_step(context):
        for row in context.table:
            assert 'Success' in row['Passed']:

Contains any `Tables`, `Text` or other associate data defined in your step.

**Passed to every function involved in a Scenario**

----

What should go into this for later Steps?

* Sessions
* Test helpers (selenium)

But ``context`` is automatically cleaned up after each **Scenario**

So make sure each of your Scenario's are isolated

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

Steps don't dictate flow
========================

Steps are just a series of ``functions``

Control flow is handled by the Scenario


----

Step Example
============

.. code:: gherkin

    Then I see there are 2 Regattas:
    | Regatta Name |
    | Nationals |
    | Europeans |

.. code:: python

    @then(u'I see there are {num_regattas:d} Regattas')
    def selenium_see_regattas(context, num_regattas):
        regattas = context.browser.find_elements_by_xpath("//section//h4[text()='Current Regattas']/..//h5")
        # Check number
        assert num_regattas == len(regattas), "Found only {} Regattas!".format(len(regattas))
        # Check rows
        found_regattas = [r.text for r in regattas]
        for row in context.table:
            assert row['Regatta Name'] in found_regattas, "{} not found in Regattas!".format(row['Regatta Name'])


----

Technical Tooling
=================

What to use to actually test?

* Behave gives us an execution framework

Free to choose what to use to drive the tests

* Selenium
* Phantom.js
* BeautifulSoup
* MyCommand.sh

----


Technology applied
==================

The hard bit

----

Lets run through an example
===========================

.. code:: Gherkin

    Feature: Regattas
      As a Regatta organizer
      I want to be able to see a list of all Regattas
      and track their races and participants
      So that I can manage the event effectively


----

Writing good Features
=====================

Clear, easy to follow but sufficiently descriptive

Clearly define:

* an initial state

* set of events 

* a desired outcome

Use a common vocabulary

Don't tie it to a technical implementation

----

.. code:: Gherkin

  Scenario: View regattas
    Given I open a webbrowser
    and I log in to the Sail Race Scoring website
    When I view all Regattas
    Then I see there are 2 Regattas:
    | Regatta Name |
    | Nationals |
    | Europeans |

----

Setting up initial state
========================

``Background`` keyword

Behaves like a Scenario definition but is executed before each and
every scenario in the current Feature

.. code:: Gherkin

  Background: I am on the Sail Race Scoring Website
    Given I open a webbrowser
    and I log in to the Sail Race Scoring website

  Scenario: View regattas
    Given I am on the Sail Race Scoring Website
    When I view all Regattas
    Then I see there are 2 Regattas:
    | Regatta Name |
    | Nationals |
    | Europeans |

----

How should I set up state?
==========================

* Eat your own dogfood / Use Public API's
* Clean state (idempotent)
* Repeatable
* DB / test specific inserts a method of last resort

----

How do I know what to check for?
================================

Check for existence

``I have X of these``

Check for state transition

``Then I see this``


Try execute your features yourself

You are the best / worst / most forgiving / error prone
BDD runner in the world!

----

Keeping Scenario's interesting
==============================

An easy to read Scenario should be a Story

Will stay relevant longer

Don't be afraid to make many features

----

Putting it all together (Demo)
==============================

"When life gives you lemons, don’t make lemonade. 
Make life take the lemons back! Get mad! 
I don’t want your damn lemons, 
what the hell am I supposed to do with these? 
Demand to see life’s manager! 
Make life rue the day it thought it could give Cave Johnson lemons!"

*Cave Jonhson, Portal 2*

----

Any questions?
==============

----

Contact Me
==========

Twitter: ``@_davem``

Github: ``github.com/dave-m``

Blog: ``david.mcilwee.me``

