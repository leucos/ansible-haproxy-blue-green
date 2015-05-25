HAProxy blue-green
==================

Backend-switching based [blue-green](http://martinfowler.com/bliki/BlueGreenDeployment.html) deployements.

This role will let you swap haproxy backends depending on the deployed color.

When used, it will create two backends: blue and green. You call this role with `blue_green_color` set to blue or green, and it will set the current color to `blue_green_color`.

It works by setting the inactive color backend to `disabled`.

The blue-green deployement process roughly follows this pattern:
- check which color is the oldest (and thus, can be deployed)
- deploy application to the corresponding color target (a directory for instance)
- point loadbalancers to the new color

Requirements
------------

Any pre-requisites that may not be covered by the ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

  - `haproxy_blue_green_front_name`: HAproxy frontend entry name to create
  - `haproxy_blue_green_loadbalancers`: Group name of loadbalancers to configure
  - `haproxy_blue_green_backend_group`: Group name of backend to add in the backend

Usage
-----

The role is supposed to be used this way from a playbook:

   - { role: leucos.haproxy-blue-green,
       haproxy_blue_green_front_name: 'myfront',
       haproxy_blue_green_loadbalancers: 'lb',
       haproxy_blue_green_backend_group: 'www',
       when: blue_green_color is defined }

`blue_green_color` can be set earlier in the playbook using `leucos.blue-green` role, which tries to guess which color is the oldest (and thus, can be redeployed).

Dependencies
------------

This role depends on:
- [leucos.haproxy-multi](https://github.com/leucos/ansible-haproxy-multi) for HAProxy configuration (using multiple configuration file)
- [leucos.blue-green](https://github.com/leucos/ansible-blue-green) for blue-green color guessing 

License
-------

MIT

Author Information
------------------

[@leucos](https://github.com/leucos)
