python-systemd
==============

python-systemd python wrapper for `systemd`_ system and session manager dbus
interfaces.

.. systemd: http://www.freedesktop.org/wiki/Software/systemd

Basic usage
-----------

Import and create a `manager`:

```
>>> from systemctl.manager import Manager
>>> manager = Manager()
```

List all units:

```
>>> for unit in manager.list_units():
...    print unit.properties.Id
...    print unit.properties.Description
...
nfs-server.service
LSB: Kernel NFS server support
virtualbox.service
LSB: VirtualBox Linux kernel module
mandi.service
LSB: Network monitoring daemon
crond.service
LSB: run cron daemon
...
```

Get an unit:

```
>>> unit = manager.get_unit('crond.service')
```

`crond` is running:

```
>>> print unit.properties.LoadState, unit.properties.ActiveState, unit.properties.SubState
loaded active running
```

Let's stop `crond`:

```
>>> unit.stop('fail')
<systemd.job.Job object at 0x7fa57ba03a90>
```

Is crond running? why I stop it!!:

```
>>> print unit.properties.LoadState, unit.properties.ActiveState, unit.properties.SubState
loaded active running
```

We want o loop!:

```
>>> import gobject
>>> gobject.MainLoop().run()
...
KeyboardInterrupt
```

Now Unit properties is updated!:

```
>>> print unit.properties.LoadState, unit.properties.ActiveState, unit.properties.SubState
loaded inactive dead
```

Let's start `crond`:

```
>>> unit.start('fail')
<systemd.job.Job object at 0x7fa57ba03950>
```

Remember we want o loop!:

```
>>> print unit.properties.LoadState, unit.properties.ActiveState, unit.properties.SubState
loaded inactive dead
```

The loop!:

```
>>> gobject.MainLoop().run()
...
KeyboardInterrupt
```

Updated!:

```
>>> print unit.properties.LoadState, unit.properties.ActiveState, unit.properties.SubState
loaded active running
```
