.. _rg_module:


rg -- manage resource groups in PowerHA cluster
===============================================

.. contents::
   :local:
   :depth: 1


Synopsis
--------

This module creates or deletes resource group in PowerHA cluster.






Parameters
----------

  name (True, str, None)
    name of the resource group.


  nodes (False, list, None)
    list of the nodes where the resource group can be started. required if resource group is created.


  startup (False, str, None)
    startup policy for the resource group. One of OHN, OFAN, OAAN, or OUDP.


  fallover (False, str, None)
    fallover policy for the resource group. One of FNPN, FUDNP, or BO.


  fallback (False, str, None)
    fallback policy for the resource group. One of NFB, or FBHPN.


  service (False, list, None)
    list of service labels for the resource group.


  application (False, list, None)
    list of application controllers for the resource group.


  volgrp (False, list, None)
    list of volume groups for the resource group.


  state (False, str, present)
    the desired state of the resource - present, absent, started, stopped. If the resource is already defined, it will not be changed.









Examples
--------

.. code-block:: yaml+jinja

    
    # create a new resource group
    - name: create a new resource group
      enfence.powerha_aix.rg:
        name: rg_oracle
        nodes:
          - node1
          - node2
        startup: OHN
        fallover: FNPN
        fallback: NFB
        service: [ 'serviceip' ]
        application: [ 'ac_ora' ]
        volgrp:
          - vg01
          - vg02
          - vg03
        state: present
    # bring resource group online
    - name: starting resource group
      enfence.powerha_aix.rg:
        name: rg_oracle
        state: started
    # bring resource group offline
    - name: stopping resource group
      enfence.powerha_aix.rg:
        name: rg_oracle
        state: stopped
    # delete an existing resource group
    - name: delete an existing resource group
      enfence.powerha_aix.rg:
        name: rg_oracle
        state: absent



Return Values
-------------

changed (always, bool, )
  set to true if the resource was changed


msg (always, str, )
  error and informational messages


rc (always, int, )
  return code of the last executed command


stdout (always, str, )
  standard output of the last executed command


stderr (always, str, )
  standard error of the last executed command





Status
------





Authors
~~~~~~~

- Andrey Klyachkin (@aklyachkin)

