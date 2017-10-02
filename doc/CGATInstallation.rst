.. _CGATInstallation:

=========================
Installation instructions
=========================

The section below describes how to install the CGAT scripts. Please
note that we can not test our code on all systems and configurations
out there. If something does not work, please try a
:ref:`CGATCleanInstall` or download a copy of the
:ref:`CGATInstallationVirtualBoxUbuntu` with all the software
installed.

Quick installation
==================
Install using conda
-------------------

The preferred methiod for installation of the CGAT code collection is using the CGAT conda channel.

Before you install, please make sure you have conda installed on your machine, if this is installed all you need to do is to type::

        # add cgat channel
        conda config --add channels cgat

        # install CGAT code in Conda's root environment
        conda install <cgat-package>

where ``<cgat-package>`` can be one of these:

* ``cgat-scripts``

* ``cgat-scripts-lite``

To install the development version of the code please follow instructions here_.


Install using pip
-----------------

The non preferred method for installation is through pip, however a number of dependancies need to be installed before the code collection will work. This is the reason we suggest you install using conda.

Installing CGAT can be straight-forward if all its dependencies are satisfied::

   pip install cgat

However, CGAT depends on numerous other python packages which themselves might require
manual intervention. Please see :ref:`ManualInstallation` for a
step-by-step installation approach.

Initialization
--------------

In order to run pipelines and code directly from the CGAT script
repository, you need to perform the following initializations::

   python setup.py develop --multi-version

This will compile all the extension modules without installing 
anything. To use, add the CGAT directory to ``$PYTHONPATH``
environment variable::

   export PYTHONPATH=$PYTHONPATH:/location/to/cgat

You might also want to run the script::

   python scripts/cgat_build_extensions.py 

to test if all the scripts with associated cython_ code compile
cleanly.

.. _ManualInstallation:

Manual installation
===================

The CGAT installation requires setuptools version 1.1 or higher
to be installed. If your system has no setuptools installed, or
an old version, please install setuptools_ first by::

   wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py -O - | python

CGAT depends on numerous other python packages not all of which
might install cleanly. Here, we give some more detailed instructions.
Generally we recommend when troubleshooting CGAT installation to do so
within a virtual environment. To create a clean environment, type::

    virtualenv --no-site-packages cgat-python
    source cgat-python/bin/activate

Now, download the list of required packages::

    wget https://raw.github.com/CGATOxford/cgat/master/requires.txt

To install the required basic packages::

    pip install -r requires.txt

.. note::
   The order in which packages are installed matters. The order	
   in :file:`requires.txt` should work, but pip might ignore that. To
   install requirements in order, try the following::
      
       while read line ; do echo $line > x ; pip install -r x; rm x; done < requires.txt

If all of that works, installing the CGAT tools should now be
straight-forward::

    pip install cgat

If you continue having problems with the installation please try the
:ref:`CGATCleanInstall` guide or download a copy of the 
:ref:`CGATInstallationVirtualBoxUbuntu` with all the software installed.

Troubleshooting
---------------

Some packages will require additional system-level packages to 
be installed. The following depencies might cause problems:

PyGreSQL
    requires postgres-devel

PyGTK
    not installable via setuptools_, install separately.

biopython_
    pip occasionally fails for biopython_. If so, try installing 
    manually.

.. _CGATCleanInstall:

CGAT Code Clean Installation
============================

In this section you will find detailed information on how to install the CGAT
Code Collection and all its dependencies inside a newly created environment.

Installation instructions for the following operating systems are available:

.. toctree::

  CGATInstallationOSX
  CGATInstallationLinux

Furthermore, we also provide different means of getting the CGAT Code Collection
pre-installed:

.. toctree::

   CGATInstallationVirtualBoxUbuntu
   CGATInstallationVagrant
   CGATInstallationDocker

.. _GalaxyInstallation:

Installing in Galaxy
====================

CGAT tools can be used through the `galaxy`_ framework. In order
to set up the CGAT tool box in you own galaxy_ instance, use the 
:file:`cgat2rdf.py` script.

The sequence of commands is:

1. Install Galaxy

2. Install CGAT 

3. Run the `cgat2rdf.py` script (see :doc:`scripts/cgat2rdf`) to
   create an xml file for inclusion into galaxy_. For example, to
   create a wrapper for `bam2stats.py` (see :doc:`scripts/bam2stats`),
   run, where ``cgat-xml`` is the location of tool xml files within
   galaxy_::

       python <cgat-scripts>cgat2rdf.py --format=galaxy <cgat-scripts>bam2stats.py > <cgat-xml>bam2stats.xml

4. Add an entry to :file:`tool_conf.xml` for the script within the
   galaxy_ distribution::

      <section name="CGAT Tools" id="cgat_tools">
          <tool file="<cgat-xml>/bam2stats.xml" />
      </section>


A list of galaxy compatible scripts is in file
:file:`galaxy.list`. This file is part of the CGAT repository and can
be used to create all wrappers in one go::

   cat galaxy.list
   | cgat2rdf.py
        --source-dir=<cgat-scripts>  --input-regex="(.*).py"
	--output-filename-pattern=<galaxy-xml>/%s.xml --format=galaxy

Within galaxy_, CGAT scripts will use samtools_ formatted genomic
sequences, which are located in the ``sam_fa_indexes`` galaxy_
resource.

.. _setuptools: https://pypi.python.org/pypi/setuptools
.. _biopython: http://biopython.org/
.. _here: CGATInstallationLinux.html