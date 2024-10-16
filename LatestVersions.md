# Latest Versions of TestCentric Packages


|         Packages       | Latest<br>Production<br>Release | Latest<br>Development<br>Release (1) | Next<br>Milestone (2) |
| ----------------------------- | --------------- | ---------------- | ------------ |
| **Recipe:**                   |                 |                  |              |
| TestCentric.Cake.Recipe       | 1.3.3           |                  |              |
| **Runner:**                   |                 |                  |              |
| TestCentric.GuiRunner         | 2.0.0-beta6     |                  |              | 
| TestCentric.Engine            | 2.0.0-beta6     |                  |              |
| TestCentric.Agent.Core        | 2.1.0           |                  |              |
| TestCentric.Engine.Api        | 2.0.0-beta6     |                  |              |
| TestCentric.Extensibility     | 3.0.2           |                  |              |
| TestCentric.Extensibility.Api | 3.0.2           |                  |              |
| TestCentric.Metadata          | 3.0.3           |                  |              |
| TestCentric.InternalTrace     | 1.2.1           | 1.2.2-dev00001   |              |
| **Agents:**                   |                 |                  |              |
| Net80PluggableAgent           | 2.5.1           |                  |              |
| Net70PluggableAgent           | 2.5.1           |                  |              |
| Net60PluggableAgent           | 2.5.1           |                  |              |
| Net50PluggableAgent           | 2.2.1           | 2.2.2-dev00001   | 2.2.2        |
| NetCore31PluggableAgent       | 2.2.0           | 2.2.1-dev00003   | 2.2.1        |
| NetCore21PluggableAgent (3)   | 2.2.0           | 2.2.1-dev00009   | 2.2.1        |
| Net462PluggableAgent          | 2.5.1           |                  |              |
| Net40PluggableAgent           | 1.1.0           | 1.1.1-dev00001   | 1.1.1        |
| Net20PluggableAgent           | 2.2.0           | 2.2.1-dev00001   | 2.2.1        |


NOTES:

1. If no dev builds are shown, it means that none have been created since the last production release.
   There may still be earlier ones, prior to the production release.
2. Next Milestone is not shown unless one is planned, even if some dev builds currently exist. This may
   be the case if we have made build improvements without fixing any bugs or adding features.
3. NetCore21PluggableAgent must be built and published manually from local machine with proper keys. 

# TestCentric Package References

| Package                                | References          |
| -------------------------------------- | ------------------- |
| TestCentric.GuiRunner<br><br><br><br>  | TestCentric.Engine<br>TestCentric.Engine.Api<br>TestCentric.Extensibility<br>TestCentric.InternalTrace
| TestCentric.Engine<br><br><br><br>     | TestCentric.Engine.Api<br>TestCentric.Extensibility<br>TestCentric.Metadata<br>TestCentric.InternalTrace
| TestCentric.Agent.Core<br><br><br><br> | TestCentric.Engine.Api<br>TestCentric.Extensibility<br>TestCentric.Metadata<br>TestCentric.InternalTrace
| TestCentric.Engine.Api<br><br>         | TestCentric.Extensibility.Api<br>TestCentric.InternalTrace
| TestCentric.Extensibility<br><br>      | TestCentric.Metadata<br>TestCentric.InternalTrace
| TestCentric.Extensibility.Api          | --
| TestCentric.Metadata                   | --
| TestCentric.InternalTrace              | --
| 
| All Pluggable Agents | TestCentric.Agent.Core<br>TestCentric.Engine.Api<br>TestCentric.Extensibility<br>TestCentric.Metadata<br>TestCentric.InternalTrace