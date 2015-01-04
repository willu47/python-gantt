How to make a Gantt graph from an Org-Mode project ?
====================================================

Follow the structure of the [example.org](file:example.org) file
----------------------------------------------------------------

### RESSOURCES

Ressources are affected to tasks by using them as tags. So the first word of each item will be the tag (Ressource1). Following charaters could be used as description.

Each ressource can contain a list of vacations which can take two forms :

* single date such as [2014-12-30 mar.]
* a timeframe such as [2014-12-30 mar.]--[2015-01-02 ven.], last day is included

### VACATIONS

Vacations item can contain a list of vacations which can take two forms :

* single date such as [2014-12-25 jeu.]
* a timeframe such as [2014-12-25 jeu.]--[2015-01-01 jeu.], last day is included

Those vacations are for all ressources.

### Projects

Projects are level 1 items other than RESSOURCES or VACATIONS. If they are tagged with the :no_gantt: tag, project will not be included.

For each project, two SVG will be generated :

* PROJECTNAME.svg : it contains the Gantt graph for the project
* PROJECTNAME_ressources.svg : it contains the graph for the ressources affectation.


For the whole projects, two graphs are generated :


* project.svg
* project_ressources.svg

### Tasks

Tasks are level 2 items. They depends from a project (level 1). Timeframe is delimited by from two of the following criteria :

* starting date : set by the SCHEDULED keyword followed by a date
* duration : set as a property, keyword "Effort", duration is defined in days !
* ending date : set by the DEADLINE keyword followed by a date

Other options are possible :

* percent done : defined as a property, keyword "PercentDone"
* dependencies : defined as a property, keyword "Depends". It relies on the tasks names. Multiples dependencies are separated by semicolons. (see [Task 4](file:example.org::*Task%204))

Installation of the scripts
---------------------------

You will need python (v2.7 or later) and some python modules :

* clize : <https://github.com/epsy/clize>
* Orgnode : <http://members.optusnet.com.au/~charles57/GTD/orgnode.html>
* svgwrite : <https://bitbucket.org/mozman/svgwrite/>

and of course those scripts :

* org2gantt.py : to transform org mode project file in my source code for my python gantt generator
* gantt.py : to transform gantt source code in SVG.

You can download them from <http://xael.org/norman/python/org2gantt/org2gantt.tar.gz>

Running the scripts
-------------------

### Transform org-mode file [example.org](file:example.org) in [example_gantt.py](file:example_gantt.py::#!/usr/bin/env%20python3)

``` bash
  python org2gantt.py example.org -g example_gantt.py
```

You can check help for options :

``` bash
  python org2gantt.py -h
```

      Usage: org2gantt.py [OPTIONS] org

      org2gantt.py

      Positional arguments:
        org, o   org-mode filename

      Options:
        -g, --gantt=STR   output python-gantt filename (default
                          sysout)(default: )
        -d, --debug       debug
        -h, --help        Show this help
        -v, --version     Show the version

      Example : python org2gantt.py TEST.org

      Written by : Alexandre Norman <norman at xael.org>

### Make SVG

If the [example_gantt.py](file:example_gantt.py::#!/usr/bin/env%20python3) was generated, it should be straitaway : just launch it...

``` bash
  python example_gantt.py
```

You should have now have those files :

* project_1ressources.svg
* project_1.svg
* project_2ressources.svg
* project_2.svg
* project_ressources.svg
* project.svg

There could be some warnings, read them, it should be easy to understand.

Interpretate the graphs
-----------------------

### Project graph

* Each project on the graph is named. A purple bar on the left groups all tasks.
* The blue vertical bar is current date (today)
* The gray days are either off work days (by default saturday and sundays) or *VACATIONS*

#### Tasks

* Each task is represented by an horizontal bar
* Name of the task is on the upper left
* Affected ressources are on the bottom left
* Dependencies between tasks are represented by dashed lines
* If the task has a upper left blueish square, the means that begining date has been influenced by constraints (vacations, dependencies...)
* If the task has a upper right blueish square, the means that ending date has been influenced by constraints (vacations, dependencies...)

### Ressource graph

* For each ressource, a line contains all tasks affected for this ressource
* On the line above, there are some markers :
    * green half square when this ressources is on vacations
    * red half square when this ressources is overcharged (more than one task at a time). In the example, task 2 and 7 are overlapping on days 22/12 and 23/12.

Licence: GPL v3 or any later version
------------------------------------

Author : Alexandre Norman (norman at xael.org)
----------------------------------------------