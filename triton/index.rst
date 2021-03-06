=================
Triton user guide
=================

Triton is the Aalto high-performance computing cluster.  It serves all
of Aalto, but is currently coordinated from within the School of
Science.  Access is free for researchers.  It is similar to the CSC
clusters, though CSC clusters are larger and Triton is easier to use
because it is more integrated into the Aalto environment.

Quick contents and links
========================

.. list-table::

   * * **Triton contents**

       * About Triton

         * `overview`
         * `usagepolicy`
	 * `acknowledgingtriton`
	 * `Aalto Science-IT <http://science-it.aalto.fi>`_ (external
	   link)

       * `Getting Help <help>`

	 * `Triton issue tracker <https://version.aalto.fi/gitlab/AaltoScienceIT/triton/issues>`_
	   (most requests here, login with HAKA)
	 * `Suggestions for good support requests
	   <https://research.csc.fi/support-request-howto>`_

       * Tutorials (start here)

	 * `accounts`
	 * `tut/connecting`
	 * `tut/storage`
	 * `tut/modules`
	 * `tut/interactive`
	 * `tut/serial`
	 * `tut/array`

       * Cluster usage details

	 * Parallel jobs (coming, for now see `usage/general`)
	 * `usage/gpu`

       * `Applications <apps>`

       For full contents, see below.

     * **News**

       **Shortcuts**

       * `Quick Reference <ref/index>`
       * Triton Cheatsheet
       * `Triton FAQ <usage/faq>`
       * `Scicomp Garage </news/garage>`

       **Scientific computing resources**

       * `SCIP – Scientific Computing in Practice courses
	 <http://science-it.aalto.fi/scip/>`_: organized
	 by Science IT. Including Triton kickstarts and many others
       * `Parallel computing <https://wiki.aalto.fi/download/attachments/65022076/parallel_computing.2012-04-12.pdf?version=1&modificationDate=1334828664000&api=v2>`_
       * `Aalto IT Services for Research <https://inside.aalto.fi/display/ITServices/IT+Services+for+Research>`_
       * `Software Carpentry <http://software-carpentry.org/>`_
	 (scientific computation basics) and
	 `Code Refinery <http://coderefinery.org/>`_ (more focused on
	 programming techniques)

       **General links**

       * `CSC <https://www.csc.fi/>`_ - Finland's academic computing center.
       * `FGCI user's guide
	 <https://research.csc.fi/fgci-user-guide>`_ at CSC: That is a
	 general Guide to FGCI resources. Triton is one of them.
       * `Taito user guide
	 <https://research.csc.fi/taito-user-guide>`_ at CSC: a
	 Triton like cluster at CSC. Similar setup, thus examples and
	 instructions can be useful.
       * `Aalto research data management information <http://www.aalto.fi/en/research/research_data_management/>`_



Overview
========

.. toctree::
   :maxdepth: 1

   overview.rst
   help.rst
   accounts.rst
   usagepolicy.rst
   acknowledgingtriton.rst
   techdetails.rst

Tutorials
=========
.. toctree::
   :maxdepth: 1

   tut/connecting.rst
   tut/storage.rst
   tut/modules.rst
   Interactive jobs - running your first command <tut/interactive>
   Running in the queue <tut/serial>
   tut/array.rst

Detailed instructions
=====================
.. toctree::
   :maxdepth: 1
   :glob:

   usage/*

.. _apps:

Applications
============

Supplied software:

.. toctree::
   :maxdepth: 1
   :glob:

   apps/*

Custom software installation:

.. toctree::
   :maxdepth: 1
   :glob:

   apps/custom/*

Reference and Examples
======================
.. toctree::
   :maxdepth: 1

   ref/index
   examples/index
