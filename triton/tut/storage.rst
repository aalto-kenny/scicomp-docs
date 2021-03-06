============
Data storage
============

In this tutorial, we go over places to store data on Triton and how to
access it remotely.

Basics
======


Triton has various ways to store data.  Each has a purpose, and when
you are dealing with the large data sets or intensive IO, efficiency
becomes important.

Roughly, we have small home directories (only for configuration
files), large Lustre (scratch and work, large, primary calculation
data), and special places for scratch during computations (local
disks, ramfs, fast but temporary)

Compare this to what is available at Aalto:

-  Aalto Linux has a separate home directory, not shared with Triton.
-  Departments can have their own shares, called variously project,
   work, teamwork, archive.  These are not on Triton except the login
   node, because they are
   not high performance enough (it just takes one person to start
   50-node job that brings it down for everyone).

Think about I/O before you start! - General notes
=================================================

When people think of computer speed, they usually think of CPU speed.
But this is missing an important factor: how fast can data get to the
CPU?  In very many cases, I/O (input/output) is the true bottleneck.
This must be considered just as much as code efficiency.

The answer is that users have a variety of needs, and a variety of
filesystems.  The following checklist aims to help you to choose the
best approach for you calculations.

-  Do you need IO in the first place?
-  E.g. checkpointing code state to disk may be unwise if the code runs
   less that couple of days.
-  Some programs use local disk as swap-space. Only turn on if you know
   it is reasonable.

Avoid many small files! Use a few big ones instead. (we have a
:doc:`dedicated page <../usage/smallfiles>` on the matter)

Summary table
=============

.. include:: ../ref/storage.rst

Home directories
^^^^^^^^^^^^^^^^
The place you start when you log in.  For user init files, some small
config files, etc.  No calculation data. Daily backup.  Usually you
want to move to scratch first.

Lustre (/scratch)
^^^^^^^^^^^^^^^^^
This is the big, high-performance, 2PB Triton storage.  The primary
place for calculations, data analyzes etc.  Not backed up.  It is
shared on all nodes, and has very fast access.  It is divided into two
parts, scratch (by groups) or work (per-user).  In general, always
change to ``$WRKDIR`` when you first log in and start doing work.

See :doc:`../usage/lustre`

Local disks
^^^^^^^^^^^
Local disks are on each node separately.  For the fastest IOs with
single-node jobs. It is cleaned up after job is finished.

See the :doc:`Compute
node local drives <../usage/localstorage>` page for further details and script
examples.

**ramfs** - fast and highly temporary storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On Triton, ``$XDG_RUNTIME_DIR`` is a ramfs, which means that it looks like
files but is stored only in memory.  Because of this, it is extremely
fast, but has no persistence whatsoever.  Use it if you have to make
small temporary files that don't need to last long.  Note that this is
no different than just holding the data in memory, if you can hold in
memory that's better.


Quotas
======

All directories under ``/scratch`` (as well as ``/home``) have quotas. Two
quotas are set per-filesystem: disk space and files number.

Disk quota and current usage are printed with the command ``quota``.
'space' is for the disk space and 'files' for the total files number
limit. There is a separate quota for groups on which the user is a
member.

::

    $ quota
    User quotas for darstr1
         Filesystem   space   quota   limit   grace   files   quota   limit   grace
    /home              484M    977M   1075M           10264       0       0        
    /scratch          3237G    200G    210G       -    158M      1M      1M       -

    Group quotas
    Filesystem   group                  space   quota   limit   grace   files   quota   limit   grace
    /scratch     domain users            132G     10M     10M       -    310M    5000    5000       -
    /scratch     some-group              534G    524G    524G       -    7534   1000M   1000M       -
    /scratch     other-group              16T     20T     20T       -   1088M      5M      5M       -

If you get a quota error, see :doc:`the quotas page <../usage/quotas>`
for the solution.


Accessing and transferring files remotely
=========================================

Transferring files to/from triton is exactly the same as any other
remote Linux server.

Remote mounting using SMB
^^^^^^^^^^^^^^^^^^^^^^^^^

By far, remote mounting of files is the easiest method to transfer files.  If you are
not on the Aalto networks (wired, ``eduroam``, or ``aalto`` with
Aalto-managed laptop), connect to the :doc:`Aalto VPN
</aalto/remoteaccess>` first.  Note that
this is automatically done on some department workstations (see
below) - if not, request it!

The scratch filesystem can be remote mounted using SMB inside secure
Aalto networks at the URLs

* scratch: ``smb://lgw01.triton.aalto.fi/scratch/``.
* work: ``smb://lgw01.triton.aalto.fi/work/$username/``.

On different operating systems:

* Linux (Ubuntu for example): File manager (Nautilus) → File →
  Connect to server.  Use the ``smb://`` URLs above.
* Windows: In the file manager, go to Computer and "Map Network
  Drive".  In Windows 10 → "This PC" → right click → "Add Network
  Location".  Use the URLs above but replace ``smb://`` with ``\\`` and
  ``/`` with ``\``.  For example, ``\\lgw01.triton.aalto.fi\scratch\``.
* Mac: Finder → Go → Connect to Server.  Use the ``smb://`` URLs above.

Depending on your OS, you may need to use either your username
directly or ``AALTO\username``


Remote mounting using sshfs
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``sshfs`` is a neat program that lets you mount remote filesystems via
ssh only.  It is well-supported in Linux, for other operating systems
check.  On Ubuntu, you can mount by "File → Connect to
server" and using ``sftp://triton.aalto.fi/scratch/work/USERNAME``.

The below is slightly more generic, and makes the ``triton_work`` on
your local computer access all files in ``/scratch/work/USERNAME``.
Can be done with other folders.

::

    mkdir triton_work
    sshfs triton.aalto.fi:/scratch/work/USERNAME triton_work

This, and the options below, only use ``ssh`` to connect so in
principle are the most generic ways to connect from anywhere, if you
set up ssh properly.

Using scp or sftp
^^^^^^^^^^^^^^^^^

You can use ``scp`` and ``sftp`` to copy your files to/from triton, and for accessing
the Triton home directory this is the only way. Note however the quite
limited disk quota in your home directory.  ``sftp`` is basically
equivalent to ``scp``, but you can find nice graphical clients for it
(they are basically the same protocol).

::

    # copying to HOME
    user@pc123 $ scp testCluster.m user12@triton:
    testCluster.m                                 100%  391     0.4KB/s   00:00    
    # copying to WRKDIR
    user@pc123 $ scp testCluster.m user12@triton:/scratch/work/USERNAME/
    ...

These both use ``ssh``, so are the most general ways to connect.

Rsync
^^^^^

Rsync is similar to scp, but is smarter at restarting files.  Use rsync
for large file transfers.  ``rsync`` actually uses ``ssh``, so
you can ``rsync`` from anywhere you can ``ssh`` from.




Accessing files from Department workstations
============================================

This varies per department, with some strategies that work from
everywhere.

These mounts that are already on workstations require a valid Kerberos
ticket (usually generated when you log in). On long sessions these might
expire, and you have to renew them with ``kinit`` to keep going.

Generic
^^^^^^^

The staff shell server ``taltta.aalto.fi`` has scratch and work mounted
at ``/m/triton``, and department directories are also in the standard
paths ``/m/{cs,nbe}/{scratch,work}/``.

NBE
^^^

Work directories are available at ``/m/nbe/work`` and group scratch
directories at ``/m/nbe/scratch/$project/``.

PHYS
^^^^

Directories available on demand through SSHFS. See the `Data
transferring <https://wiki.aalto.fi/display/TFYintra/Data+transferring>`__ page
at PHYS Intranet (accessible by PHYS users only).

CS
^^

Work directories are available at ``/m/cs/work/``, and group scratch
directories at ``/m/cs/scratch/$project/``.



Next steps
==========

Optimizing data storage isn't very glamorous, but it's an important
part of high-performance computing.  You can find some hints on the
:doc:`profiling <../usage/profiling>` page.

We have these related pages:

* :doc:`../usage/lustre`
* :doc:`../usage/localstorage`
* :doc:`../usage/quotas`
* :doc:`../usage/smallfiles`
