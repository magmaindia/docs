Install AGW
===========

Install Access Gateway on Ubuntu (Bare Metal)
---------------------------------------------

Prerequisites
-------------

To setup a Magma Access Gateway, you will need a machine that satisfies the following requirements:

- AGW_HOST: 64bit-X86 machine, baremetal strongly recommended (not virtualized). You will need two ethernet ports. We use ``enp1s0`` and ``enp2s0`` in this guide. They might have different names on your hardware so just replace ``enp1s0`` and ``enp2s0`` with your current interfaces name in this guideline. One port is for the SGi interface (default: ``enp1s0``) and one for the S1 interface (default: ``enp2s0``). Note that the ``agw_install_ubuntu.sh`` script will rename the ``enp1s0`` interface to ``eth0``.

Deployment
----------

1. Create boot USB stick and install Ubuntu on your AGW host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Download the Ubuntu Server 20.04 LTS .iso image from the Ubuntu website
- Create bootable usb using etcher `tutorial here <https://ubuntu.com/tutorials/create-a-usb-stick-on-macos>`_
- Boot your AGW host from USB (Press F11 to select boot sequence, :warning: This might be different for your machine). If you see 2 options to boot from USB, select the non-UEFI option.
- Install and configure you access gateway according to your network defaults.
- Make sure to enable ssh server and utilities (untick every other)
- Connect your SGi interface to the internet and select this port during the installation process to get an IP using DHCP.

2. Deploy magma on the AGW_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo su
    wget https://raw.githubusercontent.com/magma/magma/v1.6/lte/gateway/deploy/agw_install_ubuntu.sh
    bash agw_install_ubuntu.sh


