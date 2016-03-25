Role Name
=========

Create, configure ELBs in the Omnia framework

Requirements
------------

Intended for an Omnia admin environment.

Role Variables
--------------

See `defaults/main.yml`

Dependencies
------------

[reactiveops.get-vpc-facts](https://github.com/reactiveops/ansible-get-vpc-facts) is a dependency listed in `meta/main.yml`

Example Playbook
----------------

```
---

- name: Ensure streetjumpers app elb
  hosts: localhost
  connection: local
  gather_facts: False

  pre_tasks:
    - include_vars: "{{ item }}"
      with_items:
        - ../../../account/vars.yml
        - ../env.yml
      tags: always

    - name: set fact
      set_fact:
        layer_config: "{{ stacks.streetjumpers.layers.default_elb }}"

  roles:
    - role: reactiveops.elb-manage
      elb_manage_elb_name:           "{{ stacks.streetjumpers.layers.app.elb_name }}"
      elb_manage_elb_scheme:         "{{ layer_config.scheme }}"
      elb_manage_elb_subnets:        "{{ subnets }}"
      elb_manage_elb_listeners:      "{{ layer_config.listeners }}"
      elb_manage_elb_health_check:   "{{ layer_config.health_check }}"
      elb_manage_set_dns:            true
      elb_manage_dns_overwrite:      true
      elb_manage_dns_zone:           "{{ working_dns_zone }}"

```

Author Information
------------------

ReactiveOps
