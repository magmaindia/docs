Installation of Orchestrator with one IP instead of five
********************************************************

.. image:: photos/install.png 
   :target: https://www.youtube.com/watch?v=P_FRK38nkJI
   :alt: Alternative-text

Video by Nitin Rajput (click on the above pic to see installation of orchestrator)

First, clone the repo 

.. code-block:: bash
    
    git clone https://github.com/ShubhamTatvamasi/magma-galaxy

Install Dependant Collections
=============================

.. code-block:: bash

    ansible-galaxy collection install -U shubhamtatvamasi.magma
    ansible-galaxy collection list

Setup Ansible (Ubuntu 20.04 LTS Setup):
=======================================

.. code-block:: bash

    sudo apt-get remove --purge ansible
    sudo apt update
    sudo apt install software-properties-common
    sudo add-apt-repository --yes --update ppa:ansible/ansible
    sudo apt install ansible
    ansible-galaxy collection install shubhamtatvamasi.magma --force-with-deps

Copy your public SSH key to the host:
=====================================

.. code-block:: bash

    ssh-keygen -R 192.168.5.70
    ssh-copy-id ubuntu@192.168.5.70

Update your values in the hosts.yml file before running the playbook.
    
Change directory to deploy folder.

Deploy Magma orchestrator:

.. code-block::
    
    ansible-playbook deploy-orc8r.yml

.. note::
   After deployment is done it takes around 10 minutes to start all the magma services.

Create a new user:
==================

On the orc8r terminal, run these commands.

.. code-block:: bash

    ORC_POD=$(kubectl get pod -l app.kubernetes.io/component=orchestrator -o jsonpath='{.items[0].metadata.name}')
    kubectl exec -it ${ORC_POD} -- envdir /var/opt/magma/envdir /var/opt/magma/bin/accessc \
      add-existing -admin -cert /var/opt/magma/certs/admin_operator.pem admin_operator

    NMS_POD=$(kubectl get pod -l app.kubernetes.io/component=magmalte -o jsonpath='{.items[0].metadata.name}')
    kubectl exec -it ${NMS_POD} -- yarn setAdminPassword master admin admin
    kubectl exec -it ${NMS_POD} -- yarn setAdminPassword magma-test admin admin
