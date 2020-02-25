---
categories: GSOD
comments: true
date: 2020-02-25
featured: true
image: images/gsod/gsod_mda.png
layout: single
sidebar:
  image: images/gsod/gsod_mda_small.png
  nav: 2020-02-24-gsod-progress
splash_image: images/gsod/gsod_mda.png
tags:
  - featured
  - first
title: "Project report: A user guide for MDAnalysis"
---

This report reviews the progress of constructing [a user guide for MDAnalysis](https://www.mdanalysis.org/UserGuide/) for the inaugural [Google Season of Docs](https://developers.google.com/season-of-docs).

## MDAnalysis

[MDAnalysis](https://www.mdanalysis.org) is a Python toolkit for analysing molecular dynamics data from a number of major simulation packages. Inspired by [MDTools for Python](http://www.ks.uiuc.edu/Development/MDTools/Python/) (last updated in 1997), the package was first created in 2005 by [Naveen Michaud-Agrawal](https://github.com/nmichaud). Since then, MDAnalysis has incorporated contributions from 90 developers with [5.5k+ commits on GitHub](https://github.com/MDAnalysis/mdanalysis). The vast majority of these contributors work in science, and many of the algorithms in the code have been published in scientific literature. A growing user base regularly posts questions on the [user mailing list](https://groups.google.com/forum/#!forum/mdnalysis-discussion), which now includes 495 members. As the simulation community increasingly seeks to address issues in research accessibility, reproducibility, and interoperability, the need for flexible and open-source tools such as MDAnalysis, _by_ scientists, _for_ scientists, can only grow.

This year MDAnalysis is moving to release version 1.0 of the package with a stable API, catering to Python 2.7 and Python 3 users. Version 1.0 will be followed by version 2.0, which will be Python 3 only. While MDAnalysis is moving towards a complete, consistent API, its documentation has historically comprised many disconnected sources of information that is sometimes out-of-date. In addition, the main [existing documentation](https://www.mdanalysis.org/docs/) is a mixture of user-targeted documentation, technical API reference, and notes for developers. This means that it can be difficult for users to find information on their issues; when found, the information often includes technical notes on the internal structure of MDAnalysis, which is [then confusing](https://github.com/MDAnalysis/mdanalysis/issues/1175). The mailing list then becomes the most efficient way to work out how to use MDAnalysis, meaning that developer time is taken up answering questions about basic tasks.

## Objective

Over Google Season of Docs, I worked with my mentors [Oliver Beckstein](https://github.com/orbeckst) and [Richard Gowers](https://github.com/richardjgowers) to create [a cohesive user guide for the package](https://www.mdanalysis.org/UserGuide/), distinct from the [specific API reference documentation](https://www.mdanalysis.org/docs/). The main aims for this user guide were:

- to separate the MDAnalysis documentation into a user/developer guide, and technical reference
- to provide a first-point reference for users
- to maintain a library of tutorials for common analysis tasks
- to have working examples for each analysis algorithm from setting up the data to interpreting and visualising the results
- to resolve the most common sources of confusion from users (as ascertained from questions on the mailing list)
- to make onboarding easier for new contributors by providing an overview of how the code works

As a Ph.D. student in molecular dynamics with some knowledge of Python, I am a model user, so it was easy for me to target the user guide to myself.

## Contributions

### In numbers

- **61** [pages available](https://www.mdanalysis.org/UserGuide/)
- **33** [tutorials drafted](https://lilyminium.github.io/UserGuide/examples/analysis/README.html)
- **27** [commits](https://github.com/MDAnalysis/UserGuide/commits?author=lilyminium) to the _user guide_ repository
- **41** [commits](https://github.com/MDAnalysis/mdanalysis/commits?author=lilyminium) to the _main codebase_ repository
- **41** [issues raised](https://github.com/MDAnalysis/mdanalysis/issues?utf8=✓&q=+is%3Aissue+author%3Alilyminium+) in main repository since GSOD started (17 closed)
- **12** posts replied to in the [user mailing list](https://groups.google.com/forum/#!forum/mdnalysis-discussion) in 2020 (out of 28)

### In words

First off: the user guide is still in progress. For both personal and practical reasons (elaborated on [below](#challenges)), it is not yet possible to publish a finalised product that fulfils all the aims of the guide.

I started off by **explaining central data structures in the library**, such as the [Universe](https://www.mdanalysis.org/UserGuide/universe.html) and [AtomGroup](https://www.mdanalysis.org/UserGuide/atomgroup.html). These mostly centered around ways to create them, and documenting associated methods that were not in the API reference. The [atom selection language](https://www.mdanalysis.org/UserGuide/selections.html) was easy to refine from already very good documentation, but [the user guide page on the topology system](https://www.mdanalysis.org/UserGuide/topology_system.html) is the first **exhaustive list of topology attributes**, their associated file formats, tutorial on topology building, and list of topology-specific methods.

Similarly, the user guide maintains a complete summary of the information that can be read from all supported [**file formats**](https://www.mdanalysis.org/UserGuide/formats/index.html), with [separate pages](https://www.mdanalysis.org/UserGuide/formats/format_reference.html) on each format noting particular implementation quirks and important information. [Which information is guessed, from what, and when](https://www.mdanalysis.org/UserGuide/formats/guessing.html) is also documented for the first time, summing up discussions in ongoing issues in the code.

While the MDAnalysis community is very welcoming, the process of contributing to an open-source package with an established style can be overwhelming. I created **guides for contributing** [both to the code](https://www.mdanalysis.org/UserGuide/contributing_code.html) and [to the user guide](https://www.mdanalysis.org/UserGuide/contributing_docs.html), focusing especially on [testing](https://www.mdanalysis.org/UserGuide/testing.html). These are aimed at answering many of the questions that I had when I first joined the community, and lower the barrier for potential new contributors.

Of course, the most important part of MD*Analysis* to document is the actual analyses packaged with the code. While this is still in progress, I have already **drafted 30 notebooks addressing common analysis tasks** and illustrating usage of the analysis algorithms in MDAnalysis. A preview of these can be found at my [personal mirror of the user guide](https://lilyminium.github.io/UserGuide/examples/analysis/README.html), although they have not been reviewed yet. With 3 analysis modules remaining to be documented, these will hopefully be merged in the near future.

Not everything I contributed to the user guide consisted of explaining MDAnalysis. I also **customised the [Sphinx Read the Docs theme](https://sphinx-rtd-theme.readthedocs.io/en/stable/)** that has now been ported over to the [MDAnalysis API reference](https://mdanalysis.org/mdanalysis/). I **wrote scripts** to auto-generate some of the tables in the docs, and refined a tutorial style that took advantage of the [nbsphinx](https://nbsphinx.readthedocs.io) plugin to convert Jupyter notebooks into additional pages in the user guide. This allowed us to flesh out our examples by embedding the output generated, and interactive views of the molecules we were working on with [nglview](http://nglviewer.org/nglview/latest/api.html). Furthermore, users can both download the notebook to work through it themselves, and look through the guide online.

## Challenges

The main challenge with creating the user guide was **understanding the code** to a high enough level to explain what was going on, and to note unexpected behaviour. As much of MDAnalysis had not been documented in detail before, this involved reading a lot of code. At this point I estimate that I have at least skimmed 80% the library, including the test suite. I have also looked over all of the open issues and many closed issues in an effort to understand how and why some design decisions were made. In addition, I read the papers linked in the references; my aim for the analysis tutorials was that users would have at least a base understanding of the algorithm without needing to refer to the paper. This is mainly because the papers are not always open-access, but also because it is another barrier to understanding.

The next challenge was really an overall benefit, but still delayed progress. While I was trying to create guides and tutorials, I ran across a fair amount of **usability issues**. Some analysis classes simply had undesirable behaviour (e.g. saving unwanted files); some issues were important bugs, and others were minor but required explicit documentation so that users would be aware of this behaviour. I spent some time contributing to the main codebase to solve some of these, but have not been able to resolve them all. The remaining analysis tutorials that I have yet to complete all involve classes where I would like to improve the code for usability reasons, which is one reason that progress has been slow.

Relatedly, it was difficult trying to document behaviour during **a time of change**, when developers started cleaning up code for the upcoming release. Many small parts of the user guide have been rendered out-of-date by new code contributed after Google Season of Docs started. I also have tutorials for code that has not even yet been merged into the stable development branch. For this reason it would be shortsighted to publish the user guide before version 1.0 is released, at least.

The difficulty of remaining up-to-date with code changes, with typos in current tutorials, and with outdated tutorials, prompted me to seriously consider **the maintainability of the user guide**. MDAnalysis developers are volunteers with limited time, and it is easy to forget to update details in the user guide when modifying the code. To that end I spent some time formulating scripts to generate some of the documentation. I used the [ipython sphinx directive](https://ipython.readthedocs.io/en/stable/sphinxext.html) to generate examples whenever possible, as the ipython directive actually executes code in a stateful shell. Finally, I focused on Jupyter notebooks for the analysis tutorials and wrote a script to automatically execute them and clean up references. These three measures check that the code examples do not have breaking errors, and ensure that the user will get similar results to what they can see in the guide.

In retrospect my plan to document the entire package in a user guide, and then clean up the API reference, was very ambitious. The package comprises [70k+ lines of code](https://www.openhub.net/p/mdanalysis) and has a long history, with discussions and style decisions spread over [mailing](https://groups.google.com/forum/#!forum/mdnalysis-discussion) [lists](https://groups.google.com/forum/#!forum/mdnalysis-devel), [issues](https://github.com/MDAnalysis/mdanalysis/issues), and the [wiki](https://github.com/MDAnalysis/mdanalysis/wiki). Nonetheless, I believe that it was necessary to look at the entire library and how all the components work together to write a guide that is a cohesive and comprehensive resource. It is far from finished, but during Google Season of Docs I have laid out a base and the tools necessary for myself and others to easily add to or improve the guide.

## Now -- and then

There is still a lot of work to be done, both in the immediate future and afterwards.
There are still three analysis modules remaining to be documented. It is likely that they will be refactored first ([e.g. lineardensity](https://github.com/MDAnalysis/mdanalysis/issues/2508)) before I can finalise tutorials. Once that is done, the [analysis pull request](https://github.com/MDAnalysis/UserGuide/pull/38) can be merged. The draft tutorials on my mirrored site have already been helpful in answering mailing questions, so this will be a big step forward.

After that, there are some leftover bits and pieces to document:

- the `lib` module of lower-level or helper routines
- the `streamlines` visualisation modules
- dusting off anything in the MDAnalysis cook book for the modern code

The next major step in documentation would be cleaning up the API reference. To help with that, I have been updating [an issue with more egregious things to change](https://github.com/MDAnalysis/mdanalysis/issues/2401), but this will involve both checking for technical correctness and typos, and separating tutorials from the technical reference. In addition, the MDAnalysis package can check that a docstring's examples are up-to-date by using the `doctest` module to automate testing of code blocks in the API documentation. Moving docstrings onto this scheme will be a major undertaking.

## Learnings

It's difficult to quantify everything I learned throughout this project. Of course, I learned a lot about how MDAnalysis and its algorithms work. On the technical side, this is the first time I have ever worked with reStructuredText, Sphinx, and other documentation tools; the first time I have documented anything more complicated than a function docstring. Before GSOD, I had also never contributed to an open-source package in any significant way. Many of the organisational features (e.g. writing good tests, adding data files, tinkering with dependencies and CI, etc), and the general culture of open-source were new to me.

## Thanks for all the ~~fish~~ code review

This project succeeded thanks to the experience and help generously offered by the MDAnalysis community. Chiefly I'd like to thank [Richard](https://github.com/richardjgowers) and [Oliver](https://github.com/orbeckst) for taking a chance on my _concise_ application and mentoring me in both code and documentation issues throughout the project, but I am also extremely grateful to every one of the [MDAnalysis developers](https://github.com/orgs/MDAnalysis/teams/contributors) who took the time to review my code and offer suggestions and advice. You made this experience really fun and rewarding, and I hope to continue working with you. :-)
