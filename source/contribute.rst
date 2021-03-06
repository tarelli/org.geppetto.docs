**********
Contribute
**********

* Before reporting a bug
* Where to report a bug
* How to report a bug
* How to contribute code to Geppetto
* Geppetto contribution guidelines

Before reporting a bug
======================

#. Search issue tracker for similar issues.
#. If you are running a local install check against `deployed version <live.geppetto.org>`__ to know if it's a local problem.

Where to report a bug
=====================

* A Geppetto bug should be reported on `org.geppetto <https://github.com/openworm/org.geppetto/>`__ if the root cause is unknown.
* If the root cause is known the issue can be reported on the specific Geppetto repository, i.e. `org.geppetto.frontend <https://github.com/openworm/org.geppetto.frontend>`__.
* A list of all the sub repositories can be found `here <https://github.com/openworm/org.geppetto/blob/master/README.md>`__.

How to report a bug
===================

#. Specify the version number of Geppetto where the bug occurred (the version number is written at the top of the console)
#. Specify your browser version, operating system, and graphics card. (for example, Chrome 23.0.1271.95, Windows 7, Nvidia Quadro 2000M)
#. Describe the problem in detail. Explain what happened, and what you expected would happen.
#. If applicable provide a small test-case in the form of console commands that lead to the issue. To get the list of the commands that have been issued to Geppetto type in the console at any time `G.copyHistoryToClipboard()`
#. If helpful, include a screenshot. Annotate the screenshot for clarity.

How to contribute code to Geppetto
==================================

#. Make sure you have a GitHub account.
#. Fork the repository you want to contribute to on GitHub.
#. Make changes to your clone of the repository.
#. Ensure Geppetto contribution guidelines below are respected.
#. Submit a pull request.

Geppetto contribution guidelines
================================

* Always make your contributions for the latest `dev` branch, not `master`.
* Create separate branches per patch or feature.
* Once done with a patch / feature do not add more commits to a feature branch (pull requests are not repository state snapshots, any change you do in that branch will be included in the pull request).
* If you are adding functionality to the backend of Geppetto add unit tests to cover the functionality. 
* If you are adding functionality to the frontend of Geppetto add a QUnit test coverage. 
* If you are adding functionality for which integration testing is relevant add QUnit tests to cover the whole stack.
* Make sure all pre-exisitng JUnit tests and QUnit tests (you can run them at /GeppettoTests.html, e.g. `live.geppetto.org/GeppettoTests.html <http://live.geppetto.org/GeppettoTests.html>`__) are still passing after your changes.
* Perform a UI smoke test checking that the samples in the frontend still work.
* If you add a new feature it's good to add also an example (both for showing how it's used and for testing it still works after eventual refactorings).
* If you modify existing code (refactoring / optimization / bug fix), run relevant examples to check they didn't break or that there wasn't some performance regress.
* If some `GitHub issue <https://github.com/openworm/org.geppetto/issues>`__ is relevant to patch / feature, it's good to mention issue number with hash (e.g. `#41`) in a commit message to get cross-reference in GitHub web interface.
* Format whitespace consistently with the rest of code base (For Eclipse users you can use `this <https://github.com/openworm/OpenWorm/blob/master/eclipse/GeppettoFormatter.xml>`__ format template).
* If you create new files add `License <https://github.com/openworm/org.geppetto/blob/master/LICENSE>`__ at the top.

*Special thanks to @mrdoob for being of inspiration for this process.*
