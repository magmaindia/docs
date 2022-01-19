Access Gateway
==============

Install AGW
-----------

Install Access Gateway on Ubuntu (Bare Metal)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Prerequisites
~~~~~~~~~~~~~


To setup a Magma Access Gateway, you will need a machine that satisfies the following requirements:

- AGW_HOST: 64bit-X86 machine, baremetal strongly recommended (not virtualized). You will need two ethernet ports. We use ``enp1s0`` and ``enp2s0`` in this guide. They might have different names on your hardware so just replace ``enp1s0`` and ``enp2s0`` with your current interfaces name in this guideline. One port is for the SGi interface (default: ``enp1s0``) and one for the S1 interface (default: ``enp2s0``). Note that the ``agw_install_ubuntu.sh`` script will rename the ``enp1s0`` interface to ``eth0``.


Deploy magma on the AGW_HOST
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo su
    wget https://raw.githubusercontent.com/magma/magma/v1.6/lte/gateway/deploy/agw_install_ubuntu.sh
    bash agw_install_ubuntu.sh



