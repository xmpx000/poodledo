Introduction
============
`cycle` is a command-line tool which implements the **Cycle System** from [Time Management for System Administrators](http://www.amazon.com/Management-System-Administrators-Thomas-Limoncelli/dp/0596007833) by Thomas Limoncelli. The Cycle System, in this context, has five main activities:

- Display the tasks assigned to today's list
- Add a new task
- Complete a task
- Move a task
- Reprioritize a task

It's a lot to explain in a short blurb here, but basically, the idea is to assign your tasks to a specific day, and by the end of the day, to have taken some action with each task (either completing it, deleting it, or moving it to another specific day). `cycle` implements this system by keying off of the `duedate` field in Toodledo; when you run `cycle` by itself, it will show a list of all of your tasks which have a `duedate` of today, along with their priorities. Really though, if this sounds at all intriguing, you should read the book; Tom explains it a lot better than I ever could.

Requirements
============

- Python 2.5+ (or, an older version with the ElementTree module installed)
- < Python 3 (working on this)

In addition, `tdcli` requires the following Python modules (both available by `pip install`):

- [parsedatetime](http://code.google.com/p/parsedatetime/) v0.8.7
- [python-dateutil](http://labix.org/python-dateutil) v2.1

Installation
============

    make install

Usage
=====
Running `cycle` by itself lists the tasks which are due today:

    $ cycle
    1: (A) Write README for cycle

`cycle -d` lists the tasks due on a specific day:

    $ cycle -d tuesday
    1: (D) Tuesday's demonstration task

This uses the [parsedatetime](http://code.google.com/p/parsedatetime/) library, which understands most human-readable strings ("tomorrow", "next wednesday", "june 12", etc.).

`cycle -a` adds a new task, due today:

    $ cycle -a Create a demonstration task
    $ cycle
    1: (A) Write README for cycle
    2: (D) Create a demonstration task

`cycle -p` reprioritizes a task:

    $ cycle -p 2 B
    Reprioritizing task 'Create a demonstration task' to B
    $ cycle
    1: (A) Write README for cycle
    2: (B) Create a demonstration task

`cycle -m` moves a task to another day (tomorrow by default):

    $ cycle -m 2
    Moving task 'Create a demonstration task' to tomorrow
    $ cycle -d tomorrow
    1: (B) Create a demonstration task
    $ cycle -m 7 next wednesday
    Moving task 'improve poodledo' to next wednesday
    $ cycle -d next wednesday
    1: (A) improve poodledo

`cycle -c` marks a task as complete:

    $ cycle -c 1
    Marking task 'Write README for cycle' as complete

Notes
=====
- In order to use this script, you will need to [get your own API token](http://api.toodledo.com/2/account/doc_register.php). I have not included mine in the code. Add it to `ApiClient.__init__`.
- Pull requests always welcome!

License
=======
`cycle` is released under a **BSD License**. See the LICENSE file for details.

Contact
=======
You can email me at comptona@gmail.com.

To report bugs or request features, please use the **[Issues](https://github.com/handyman5/poodledo/issues)** feature.

Old News
========
2012-06-26
----------
I made a few more improvements to the `cycle` tool:

- I improved date handling for the "--day" and "--move" arguments to better support date strings with spaces in them (like "next thursday").
- I added support for moving all of the tasks on a day by specifying '*' as the task argument. (Just specifying an asterisk gets eaten by the shell.)
- Updated "--priority" to support all five priority levels.
- Added a priority cutoff filter to the config file; by specifying "priority = -1" (or higher) in the \[filter\] section of your config file, `cycle` will hide tasks with a lower priority than that.

2011-11-16
----------
I made several changes to `cycle` which have made it more useful to me:

- `cycle` now supports a "-t" parameter; it will only print tasks with that tag. You can also add a section to the config file called "[filter]" and a key there called "tag" which will have the same effect. This way you can keep your home and work to-do lists separate if you want. Also, newly created tasks will carry the currently-defined tag.
- `cycle` now supports combining the -a (add) and -c (complete) arguments; it will create a new task and immediately close it, to aid in keeping track of completed, unplanned work.
- Tasks now sort by duedate and duetime, in addition to by priority.
- The "-d" parameter now supports a special "week" value, which sets the range of displayed tasks to ("last sunday", "saturday") inclusive. This violates the Cycle System, but I find I need the view.

I also added a bit of error checking.
