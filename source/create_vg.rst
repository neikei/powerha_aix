Adding a new file system into IBM PowerHA Cluster using Ansible
===============================================================

.. code-block:: yaml

   ---
   - name: test playbook - create volume group and file system
     hosts: node1
     gather_facts: false
     vars:
       - ldev: 25B6
       - vg: vg01
       - lv: lvora
       - mountpoint: /ora
       - rg: rg_ora
     tasks:
       - name: find disk for vg
         enfence.powerha_aix.pv_info:
           ldev: "{{ ldev }}"
         register: pv
       - name: stop if disk is not found
         ansible.builtin.fail:
           msg: disk for the volume group is not found
         when: pv.pv | length == 0
       - name: create a shared volume group
         enfence.powerha_aix.vg:
           name: "{{ vg }}"
           nodes:
             - node1
             - node2
           rg: "{{ rg }}"
           volumes: "{{ pv.pv }}"
         when: rh_result.changed
       - name: sync cluster config
         enfence.powerha_aix.cluster:
           name: cluster1
           fix: true
       - name: create logical volume
         enfence.powerha_aix.lv:
           name: "{{ lv }}"
           vg: "{{ vg }}"
           size: 10
           unit: gb
       - name: create file system
         enfence.powerha_aix.fs:
           name: "{{ mountpoint }}"
           lv: "{{ lv }}"
       - name: sync cluster config
         enfence.powerha_aix.cluster:
           name: cluster1
           fix: true
           state: synced
