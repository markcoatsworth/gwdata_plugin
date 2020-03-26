# gwdata_plugin
HTCondor file transfer plugin for the LIGO Data Replicator (LDR) service. Uses
the [GWDataFind](https://github.com/duncanmmacleod/gwdatafind) tool to request
a list of urls of Gravitational-Wave Frame (GWF) files, then uses 
[PycURL](http://pycurl.io/) to transfer each file.

## Usage
To use this plugin, simply drop the `gwdata_plugin.py` file in the same 
directory as your HTCondor job submission files. 

Your `transfer_input_files` line should be a URL with the `gwdata://` prefix
and the following format:

    gwdata://<gwdatafind-server-host>?observatory=<...>&type=<...>&s=<###>&e=<###>

You also need to include the following line so that HTCondor knows to use this 
plugin for `gwdata://` URLs:

    transfer_plugins = gwdata=gwdata_plugin.py

## Example
An example submit file might look like:

    executable = gwdata-test.sh
    output = gwdata-test.out
    transfer_plugins = gwdata=gwdata_plugin.py
    transfer_input_files = gwdata://datafind.gw-openscience.org?observatory=H&type=H1_GWOSC_O2_16KHZ_R1&s=1166311424&e=1166479360
    should_transfer_files = YES

    queue

## Requirements

The HTCondor execute node that runs this plugin requires the following:

* Python 3
* Python gwdatafind module
* Python pycurl module
