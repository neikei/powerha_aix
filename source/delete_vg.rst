Removing volume group from IBM PowerHA Cluster using Ansible
============================================================

.. code-block:: yaml

   ---
   - name: Remove volume group
     hosts: node1
     gather_facts: false
     vars:
       vg: vg01
       lv: lvora
       mountpoint: /ora
       rg: rg_oracle
     tasks:
       - name: Remove shared volume group from resource group
         enfence.powerha_aix.vg:
           name: "{{ vg }}"
           rg: "{{ rg }}"
           state: rgremove
       - name: Sync cluster config
         enfence.powerha_aix.cluster:
           name: cluster1
           fix: true
           state: synced
       - name: Delete file system
         enfence.powerha_aix.fs:
           name: "{{ mountpoint }}"
           state: absent
       - name: Delete shared volume group
         enfence.powerha_aix.vg:
           name: "{{ vg }}"
           state: absent
