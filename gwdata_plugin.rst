gwdata_plugin
=============

The **gwdata_plugin** is an HTCondor file transfer plugin used to retrieve
files reference by the LIGO Data Replicator (LDR) service. It uses the
`gwdatafind <https://github.com/duncanmmacleod/gwdatafind>`_ tool to request
a list of urls of Gravitational-Wave Frame (GWF) files, then uses 
`PycURL <http://pycurl.io/>`_ to transfer each file.

Usage
-----
To use this plugin, simply drop the ``gwdata_plugin.py`` file in the same 
directory as your HTCondor job submission files. 

Your ``transfer_input_files`` line should include a URL with the `gwdata://`
prefix and the following format:

::

    gwdata://<gwdatafind-server-host>?observatory=<...>&type=<...>&s=<###>&e=<###>

This URL supports the following arguments:

* ``observatory``: Single-character name of the site (observatory) to match.
* ``type``: Name of frame type to match
* ``s``: Integer GPS start time of query
* ``e``: Integer GPS end time of query
* ``metadata_file``: If metadata was requsted, name of the output file. Defaults to ``metadata.txt``.
* ``cache``: Metadata cache type. Possible values are ``lal``, ``lal-cache``, ``frame``, ``frame-cache``

You also need to include the following line in your job submit file so that 
HTCondor knows to use this plugin for `gwdata://` URLs:

::

    transfer_plugins = gwdata=gwdata_plugin.py

Example
-------

An example submit file might look like:

::

    executable = gwdata-test.sh
    output = gwdata-test.out
    transfer_plugins = gwdata=gwdata_plugin.py
    transfer_input_files = gwdata://datafind.gw-openscience.org?observatory=H&type=H1_GWOSC_O2_16KHZ_R1&s=1166311424&e=1166479360
    should_transfer_files = YES

    queue

Requirements
------------

The HTCondor execute node that runs this plugin requires the following:

* HTCondor 8.8+
* Python 3
* Python `gwdatafind <https://github.com/duncanmmacleod/gwdatafind>`_ module
* Python `pycurl <http://pycurl.io/>`_ module

The HTCondor submit machine sending this plugin with a job requires the following:

* HTCondor 8.9.2+
