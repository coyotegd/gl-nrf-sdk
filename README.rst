nRF Connect SDK: sdk-nrf (coyotegd fork)
#########################################

.. contents::
   :local:
   :depth: 2

This repository contains the core of nRF Connect SDK, including subsystems,
libraries, samples and applications.
It is also the SDK's west manifest repository, containing the nRF Connect SDK
manifest (west.yml).

Why this fork exists
********************

This is a personal fork of the `GL-iNet GL-NRF-SDK
<https://github.com/gl-inet/gl-nrf-sdk>`_ (itself a fork of Nordic's
nRF Connect SDK v2.2.0), maintained at
``https://github.com/coyotegd/gl-nrf-sdk``.

The fork was created for one reason: the upstream ``west.yml`` manifest
hard-codes the GL-iNet application repository
(``gl-thread-dev-board``) as a west module.  To build customised
firmware without depending on GL-iNet's servers, the manifest was
patched on the ``v2.2.0-glinet`` branch to point that module at the
companion fork ``coyotegd/gl-thread-dev-board`` instead.

How it is used
**************

This repo is the **west manifest** for a local nRF Connect SDK build
workspace at ``/volume2/nrf-build``.  The workspace was initialised
with::

    west init -m https://github.com/coyotegd/gl-nrf-sdk \
              --mr v2.2.0-glinet /volume2/nrf-build
    west update   # fetches ~45 modules into /volume2/nrf-build

Firmware is then built inside the
``nordicplayground/nrfconnect-sdk:v2.2-branch`` Docker container::

    west build -b gl_nrf52840_dev_board   # from glinet/gl-dev-board-over-thread/

The resulting ``build/zephyr/app_update.bin`` is flashed OTA to the
GL-iNet Thread Dev Boards (TDB1/TDB2) via ``mcumgr`` over the Thread
network using the S200 router as a relay.

Customisations in this workspace
---------------------------------

All application-level changes live in the companion repo
``coyotegd/gl-thread-dev-board`` (branch ``main``).  This SDK fork
itself only carries the manifest patch; no Zephyr, NCS, or board
files have been altered beyond what GL-iNet originally added.

Documentation
*************

Official documentation at:

* Latest: http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest
* All versions: http://developer.nordicsemi.com/nRF_Connect_SDK/doc/
