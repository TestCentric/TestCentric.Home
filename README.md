# TestCentric Home

This repo is the home base for the TestCentric organization. It contains general info about all the TestCentric projects. We also use it for plans and discussions that go across individual projects and repositories. This document gives a general description of what the TestCentric group is all about.

## Test-Centric Development

The name of the group comes from the phrase "Test-Centric Development." I use the term when conducting training or coaching teams to describe an approach where pretty much everything a team does has a test of some kind behind it. The TestCentric group is focused on developing tools and techniques that support that approach to software development.

## Overview of Software Projects

Here on GitHub, the TestCentric organization aims to produce tools. Everything at this point is Open Source. There may be commercial versions of the same tools as well as new commercial tools at some time in the future.

Much of the tooling that is planned will leverage my own work on NUnit in the past but will hopefully go beyond what has been done before. The following are the projects now underway or being actively considered.

### TestCentric GUI

The initial release of the GUI will look and work very much like the NUnit V2 GUI. It will support NUnit 3 tests and - using the standard NUnit extension - NUnit V2 tests as well. This project is under way and version 1 is currently in alpha.

The separate Experimental GUI project has now been terminated but the code for it has been added to the TestCentric GUI. The TestCentric GUI will be released with both the standard GUI and the experimental version. The experimental GUI is considered pre-alpha software at this point but many or most of its features will be moved into the standard GUI and it is likely to become the primary GUI when we get to version 2.

For version 2, I also envision supporting multiple test frameworks, including...

### TestCentric Framework

This is still on the drawing board, so I won't say much about it. 

Generally, I'm thinking of carrying forward some of NUnit's syntactic innovations in a slightly expanded form and separating the assertion facility from the test framework itself. I would also like to allow users to choose among alternate sets of features for different kinds of testing - micro-tests versus functional testing, for example.

Many of the ideas that have been germinating around this new framework were nurtured by folks in the Lonely Coaches Sodality group.

### TestCentric Extension for Visual Studio

This is a bit more futuristic and will build on the experimental GUI to create a true VS extension (not an adapter) for running tests. This will allow the user to have the same views of tests either within VS or in a standalone GUI.

I have not yet decided whether this will be Open Source, commercial or dual-licensed using two different versions.

### TestCentric Test Engine

I'll be evaluating whether the NUnit engine can be used to carry out all the goals of the other projects. If necessary, a separate engine will be created.

