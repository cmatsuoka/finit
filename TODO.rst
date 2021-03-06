==============================================================================
                                    TODO
==============================================================================
Unsorted TODO list of features and points of investigation that could be
useful for improving finit.  But what is an improvement to finit?  Well,
similiar to launchd, the goal of finit is to be a FAST replacement for:

* init
* inetd
* crond
* watchdogd

The main goal is to be FAST. Secondary goals are to be: small, simple
and with a very low dependency to external packages.

Priority
--------
The intent of finit is to monitor not just processes, but also to
be able to supervise the entire system and the behavior of certain
processes that declare themselves to be finit compliant.

* Integrate the ingenious libev to handle all events: signals, polling
  fd's, etc.  Because "Event driven software improves concurrency" --
  Dave Zarzycki, Apple.  See the `launchd video`_ for more info!
* Add support for process and system supervision.  Which could
  be an API wrapping, e.g., TIPC and loadavg.
* Add support for a /dev/watchdog plugin to replace watchdogd
* Integrate watchdog plugin with process/system supervisor to
  reboot the system when a process stops responding, or when
  restarting it is imposible, as well as when system load gets
  too high.

Configuration
-------------
* Add support for "init <q | reload-configuration>" and SIGHUP to
  reload finit.conf, but also
* Add support for inotify to automatically reload finit.conf

Runlevels
---------
* Add support for "runlevel N" to finit.conf
* Improve support for runlevels: startup, running (services from
  finit.conf), and shutdown as well as hooks when switching RL,
  startup at RL 1 and a default run level.
* Implement initctl stop|start|restart|reload|status <SVC> and
  service <SVC> stop|start|restart|reload|status on top
* Add PRE and POST hooks for when changing runlevels
* SysV init in Debian uses an rcS.d/ for scripts that should run once
  at boot. These scripts are run before the actual runlevel set in
  inittab. In finit we could use "task [S]" and "boot [S]" for this.
* Add support for a new runlevel 'S' that is started before all other
  services in the selected runlevel.

Services and Tasks
------------------
Both the "task" and "boot" stanzas below are like "service", share the
same mechanism for registering callbacks.  These callbacks can be used
to check a configuration db and return 1 (RUN), 2 (STOP), or 3 (RELOAD).

* Add support for "task [RUNLVL] ..." similar to services, but
  one-shot. Needs a trigger/when
* Add support for "boot [RUNLVL] ..." similar to services, but
  waits for completion before continuing with the next line
* Add support for service/task dependencies
* Add support for "init list" to show running services

Miscellaneous
-------------
* Add support for "init -v,--version | version"
* Cleanup move sources from top-level directory to src/ and include/
* Make sure to install queue.h to $(PREFIX)/include/finit/queue.h

Documentation
-------------
* Write man pages for finit and finit.conf, steal from pimd man pages...
* Update Debian finit.conf example with runlevels, tty/console and
  dependency handling

Investigation
-------------
* `FHS changes`_ affecting runtime status, plugins, etc.

.. _`launchd video`: http://www.youtube.com/watch?v=cD_s6Fjdri8
.. _`FHS changes`: http://askubuntu.com/questions/57297/why-has-var-run-been-migrated-to-run
..
.. Local Variables:
..  mode: rst
..  version-control: t
.. End:
