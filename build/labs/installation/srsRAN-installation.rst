Installation of srsRAN
**********************

First, we need to clone this repo (https://github.com/ShubhamTatvamasi/srsRAN-demo)
           
Prerequisites
=============

You already have a Magma AGW and Orchestrator running and have access to the NMS in order to make changes. This will include installing many other dependencies, some of which will be used in this demo.
Importantly, you will probably already have Ansible installed
And have Vagrant installed along with Virtualbox
When finished with these, you will also need an additional Ansible dependency which you can download with:

.. code-block:: bash

    ansible-galaxy collection install community.docker

Preparing Magma for UE traffic in Your NMS
==========================================

Visit your NMS at something like: ``https://orgdomain.nms.orc8r.yourdomain.com``

This assumes that you have already created and deployed the Magma AGW and Orchestrator including an organizational subdomain. Visit your NMS at something like: https://orgdomain.nms.orc8r.yourdomain.com
Then go to Traffic -> APNs -> Create New APN

.. image:: photos/new-apns.png
  :width: 800
  :alt: Alternative text

Create an APN plan with an id of ``default``.
Then, go to the Network tab and edit the EPC section:

.. image:: photos/network.png
  :width: 800
  :alt: Alternative text

In this window change the MCC, MNC and TAC to the following:
- MCC -> 001
- MNC -> 01
- TAC -> 7
Next, upload the subscribers.csv IMSI list in this repository to your NMS UEs under Subscriber -> Config -> Add Subscriber:

.. image:: photos/new-apns.png
  :width: 800
  :alt: Alternative text

Then press the upload button, select the file, and save it afterward:

.. image:: photos/upload-and-save.png
  :width: 800
  :alt: Alternative text


Add Your SSH Public Key to the srsRAN VM for Ansible
====================================================

Update hosts, bind_ip, mme address in hosts.yml file
====================================================

Deploy srsRAN
=============

Now we can use Ansible to install Docker within the Vagrant VM, build the srsRAN images and then kick off a demo.
Install Docker and srsRAN on the srsRAN Vagrant VM with Ansible:

.. code-block:: bash

    ansible-playbook install-everything.yaml

This should automatically start the process of connecting to your AGW with one of the UEs.

Verify setup
============

To verify that the process was successful check the docker logs from inside the srsRAN Vagrant VM:

.. code-block:: bash

    docker logs srsue -f