Adding a new file system into IBM PowerHA Cluster using Ansible
===============================================================

.. code-block:: yaml

   ---
   - name: Create volume group and file system
     hosts: node1
     gather_facts: false
     vars:
       ldev: 25B6
       vg: vg01
       lv: lvora
       mountpoint: /ora
       rg: rg_oracle
       nodes:
         - aix101
         - aix102
     tasks:
       - name: Find disk for vg
         enfence.powerha_aix.pv_info:
           ldev: "{{ ldev }}"
         register: pv
       - name: Stop if disk is not found
         ansible.builtin.fail:
           msg: disk for the volume group is not found
         when: pv.pv | length == 0
       - name: Create a shared volume group
         enfence.powerha_aix.vg:
           name: "{{ vg }}"
           nodes: "{{ nodes }}"
           rg: "{{ rg }}"
           volumes: "{{ pv.pv }}"
       - name: Sync cluster config
         enfence.powerha_aix.cluster:
           name: cluster1
           fix: true
           state: synced
       - name: Create logical volume
         enfence.powerha_aix.lv:
           name: "{{ lv }}"
           vg: "{{ vg }}"
           size: 9
           unit: gb
       - name: Create file system
         enfence.powerha_aix.fs:
           name: "{{ mountpoint }}"
           lv: "{{ lv }}"
       - name: Sync cluster config
         enfence.powerha_aix.cluster:
           name: cluster1
           fix: true
           state: synced
