.. pysmm documentation master file, created by
   sphinx-quickstart on Mon Apr 09 16:03:43 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

.. _welcome:

PYthon Sentinel-1 soil-Moisture Mapping Toolbox (PYSMM)
===============================================

This package acts as an interface to Google Earth Engine for the estimation of surface soil moisture based
on Copernicus Sentinel-1 intensity data. It is meant as a supplement to the following publication: *Greifeneder, F.,
C. Notarnicola, W. Wagner. Machine learning based mapping of high resolution, global surface soil moisture content.*
The estimation of soil moisture is based on a Support-Vector-Regression machine learning approach. The model training
was performed based on in-situ data from the International Soil Moisture Network (ISMN). PYSMM all processing steps
for spatial and temporal mapping of surface soil moisture are fully executed online on GEE - none of the input data-sets
needs to be downloaded.

Acknowledgements: This study was made possibly through funding within the Horizon 2020 project "Ecopotential: Improving
Future Ecosystem Benefits through Earth Observations"

.. image:: _static/logoEURAC.jpg
.. image:: _static/ecopo_small.png

.. _installation

Installation
============

Most of the data processing is executed on-line on Google Earth Engine.
Therefore, the execution of this script requires a Google account and access to Google Earth Engine -
we are working on an updated version that will utilize a GEE Application Key rather than a personal account.

**Installation of the Google Earth Engine API**

To allow the script to talk to Google Earh Engine the API has to be installed. Please follow the instructions at
this link `GEE API`_

.. _GEE API: https://developers.google.com/earth-engine/python_install_manual

**Installation of the Google Drive API**

After the computation inside Google Earth Engine is finished, the results are exported to your Google Drive.
To let the script access and download the results to you local computer, the Google Drive API
has to be installed as well. Please follow the instructions here: `Google Drive API`_

.. _Google Drive API: https://developers.google.com/drive/v3/web/quickstart/python

As described in the manual, for the first run, the authentication can be initiated by running the ``quickstart.py``
script. To enable the download of data please modify the following line of the script:

**SCOPES = 'https://www.googleapis.com/auth/drive.metadata.readonly' --> SCOPES = 'https://www.googleapis.com/auth/drive'**

Install PYSMM by running:

``pip install pysmm``

or

``git clone https://gitlab.inf.unibz.it/Felix.Greifeneder/pysmm``


``python setup.py install``


.. _API

API Documentation
=================

get_map()
---------

To produce a soil moisture map of any geographic extent use the following function::

    from pysmm.derive_SM import get_map
    get_map(*minlon*, *minlat*, *maxlon*, *maxlat*,
            'path/to/outdir/',
            samping=100,                            # specify desired output resolution
            year=None, month=None, day=None,        # define a date (optional)
            tracknr=None,                           # define the Sentinel-1 track (optinal)
            tempfilter=True,
            mask='Globcover',
            masksnow=True,
            overwrite=True)

* in case *year*, *month*, and *day* are not specified, the entire time-series will be extracted

* if *tempfilter=True*, temporal speckle filtering is applied to reduce speckle noise

* mask='...' specifies the land-cover classification that is used for masking the output. *Globcover*
  provides global coverage. For Europe and North America, *Corine* and *USGS*, respectively, offer more precise
  information.

* if *masksnow=True*, an automatically generated wet-snow mask is applied to the output

* *overwrite=True* allows to specify whether existing files should be overwritten or skipped.

* the output of ``get_map()`` is directly downloaded to *'path/to/outdir/'*

get_ts()
--------

To derive the soil moisture time-series of a single location use ``get_ts()``::

    from pysmm.derive_SM import get_ts
    sm_ts = ge_ts(*lon*, *lat*,
                  '/path/to/working_dir/',
                  tracknr=None,                 # define an optional Sentinel-1 track-nr filter
                  footprint=50,                 # specify the footprint of the extracted time-series
                  masksnow=True,
                  calc_anomalies=True,
                  create_plots=True)

* The output of *get_ts* is a pandas time-series

* if *masksnow=True*, an automatically generated wet-snow mask is applied to the output

* if *calc_anomalies=True*, anomalies are generated in addition to absolute soil moisture values.
  For details about the anomaly computation, see: *Greifeneder F, E Khamala, D Sendabo, W Wagner, M Zebisch, H Farah,
  C Notarnicola. Detection of soil moisture anomalies based on Sentinel-1. Physics and Chemistry of the Earth (submitted,
  March 2018)*

* if *create_plots=True*, time-series plots are created and saved to *'path/to/working_dir'*

.. _contribute

Contribute
==========

Issue Tracker: https://gitlab.inf.unibz.it/Felix.Greifeneder/pysmm/issues

Source Code: https://gitlab.inf.unibz.it/Felix.Greifeneder/pysmm

.. _contact

Contact
=======

If you are having issues, please let us know.
Mail to: felix.greifeneder@eurac.edu

.. _license

License
=======

The project is licensed under the GNU GENERAL PUBLIC LICENSE Version 3
