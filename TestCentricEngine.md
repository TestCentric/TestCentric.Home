# Creating a Test Engine for TestCentric

This document describes the origin of the NUnit TestEngine and its current features and development as compared to the needs of the TestCentric GUI. It lays out the options available in developing a new engine or modifying the NUnit engine to meet those needs and lays out a direction for future development. ~~Charlie Poole

## Background

In fall of 2006, at the Mono conference in Cambridge, Massachusets, I gave a presentation on _Mono and NUnit_. At that time, NUnit 2.4 was about to come out with many new features and I was starting to think about what it would mean to move ahead with a new major release: NUnit 3.0.

Among other features, I called out this one...

      * Separation of NUnit backend from Runners?

Note the question mark. I wasn't sure we could do it or what it would mean. However, this was the first public mention, at least that I can remember, of what was to become the NUnit Test Engine.

In 2008, again at the Mono Summit conference in Madrid, my talk was entitled _Mono and NUnit : One year later_. It contained the same, bare bullet point. However, in addition, at a Birds of a Feather session, I presented the design for what was to be the __NUnit 3.0 Extended Test Platform__.

This was a three-layer architecture, consisting of runners, test engine and framework. Here's what it looked like...

![NUnit XTP Architecture - 2008](nunit-xtp-2008.png)

Many of the individual items in this diagram are no longer relevant today, but the overall design was the basis for NUnit 3.0.

## Terminology

It's surprising how easily development of an application can be derailed if terms used are not clearly defined and used in the same way by all team members. Here, I'll make some attempt to define the terms I'm using.

 * **Runners:** At the architectural level, a __runner__ is an __application__ that is used by developers to discover and execute tests. The word is overloaded at other levels. For example, the engine contains classes that are called runners because of the function they perform. As programmers and humans, we are wired to handle overloading of terms, so hopefully that shouldn't be a problem. The __TestCentric GUI__ is an example of a __runner__.

 * **Engine:** The term **engine** refers to the common software, described in the first section, that is used by all runners to discover and execute tests. It provides a set of services to every runner in order to facilitate common behavior in the running of tests. The term is typically used in two different ways, referring to either the __engine assembly__ itself or the entire __engine layer__ as shown in the architectural diagram. In this document, I'll try to use it consistently to refer to the __engine assembly__ or qualify it if necessary.

 * **Agent:** Test __agents__ form an important part of the __engine layer__. Agents are responsible for actually discovering and executing tests. They are logically distinct from the __engine assembly__ because they generally need to be built for a greater variety of platforms.

 * **Framework:** A **test framework** is designed to run tests written against a particular API. The **framework** is generally the only component that is referenced by the user's test assembly. Frameworks represent the lowest of the three architectural layers.

## NUnit Test Engine

The current NUnit Test Engine layer is an implementation of the original design, with some changes made as time has gone on. It consists of the following assemblies:

* **nunit.engine.api.dll** defines the interfaces used by runners to access the engine. It is maintained at the same 3.0.0.0 version even as other assemblies are re-versioned and is only modified by adding new things.
* **nunit.engine.dll** is the implementation of the engine. It also contains the implementation of cross-process and in-process agents and their associated runners.
* **nunit-agent.exe** and **nunit-agent-x86.exe** are console apps that serve as cross-process agents when invoked by the engine. They reference the engine code and use configuration options to enable only those parts of the engine assembly that are intended for use by an agent.

It may seem odd that the NUnit engine assembly is used for two purposes: to function as an engine and to provide services as part of an agent process. This was initially done for expediency, with the plan of splitting the code into two assemblies, one related to engine functions _per se_ and the other containing the code needed by an agent. Unfortunately, this is a rather large amount of technical debt that was never paid off.

## TestCentric GUI Requirements

Currently, the TestCentric GUI is using a modified build of the NUnit engine. So far, the modifications are minor. In this section, I want to examine some key features we will need in the near future, which are not currently provided by the NUnit engine.

1. The Gui should be able to run tests on other platforms than .NET Framework, such as .NET Core. It should, in fact, be able to load and run multiple assemblies that use different platforms. Currently, that's not possible because it requires using two different builds of the engine in the same process.

2. The GUI chocolatey package should reference the engine package, rather than including all the engine assemblies in it's own package. This is not currently possible because of how the engine itself is packaged.

3. The engine should support extensions to extensions. An issue exists for this in the NUnit project. Without that support, it's not possible for any component, which accepts extensions, to be itself an extension. Most notably affected by this are Services, which can't be added as extensions but must be coded in the engine assembly itself.

4. The engine should return proper summary tests for each supported project that is run. An issue exists for this in the NUnit project. Currently, there is adhoc code in the GUI to add this information to the visual display but it is, of course, not present in the result XML.

5. In order to communicate between the engine and agents running under multiple platforms and runtimes, new communications channels are needed. Currently, the engine only communicates across processes and AppDomains using .NET Remoting. There is an existing issue in the NUnit project intended to replace remoting. However, this will still leave the NUnit engine with only a single means of communicating with engines.

6. The engine should support extensions to the GUI itself as well as to other runners. This was part of the original architectural design and requires the ExtensionService to examine multiple roots (assemblies) rather than just the nunit.engine and nunit.engine.api assemblies. It calls for an API whereby the user can specify the roots to be examined and, in turn, will necessitate a more dynamic approach to loading and unloading extensions. Currently, extension points and extensions are detected once, upon initialization of the service, and are not allowed to change in the course of a run. There is no current issue in the NUnit project about this.

All of these items are relatively major and some of them may be out out of scope for NUnit. Up to now, the NUnit test engine has been going in a different direction, based on a separate engine build for each platform. That could change,of course, but I think TestCentric will need it's own engine sooner or later.

The question of timing is important. TestCentric GUI 1.0 has been released using the NUnit engine with minor modifications. So, essentially, we already have our own engine. For subsequent releases, I think we are likely to deviate further. That said, if we are able to make some changes in the NUnit engine before splitting off completely, we'll be able to retain a greater degree of compatibility.

## TestCentric Engine

The following is just a sketch of the changes we will need to make to the existing NUnit engine. Details will come, as usual, as we implement it.

1. This will be a new repository under the TestCentric organization.

2. The console project and tests will be removed, along with all their history.

3. The engine assembly nunit.engine.dll will be split into two separate assemblies, one containing engine functions and the other agent and common functions. Based on a spike implementation, this will require some changes to how extension points are located, since extensibility is needed in both assemblies. The engine assembly will reference the agent assembly and the agent executables will reference the agent assembly.

4. Assemblies will be renamed using a TestCentric designation at this point, in order to avoid having assemblies with the same name that don't share the same functionality.

5. The .NET standard 1.6 build of the engine will be dropped. The other builds will remain the supported version levels will probably increase in the future. The .NET Standard 2.0 build is likely to become a .NET Core build but that decision requires more analysis. Note that the engine assembly will now only need to support platforms in which the runners initially execute rather than all the target platforms under which tests are run.

6. The agent dll will retain all the same builds that it currently has since it has to be loaded in all platforms and runtimes in which we want to run tests.

7. At this point, we will switch the GUI to use the new engine, hopefully for it's 1.1 release, and continue development from there.
