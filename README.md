Ansible Role: NTP
=================

An Ansible role that installs [NTP][ntp] and configures it.


Table of Contents
-----------------

<!-- toc -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
  * [NTP Servers](#ntp-servers)
  * [Extra Restriction](#extra-restriction)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Top-Level Playbook](#top-level-playbook)
  * [Role Dependency](#role-dependency)
- [License](#license)
- [Author Information](#author-information)

<!-- tocstop -->


Requirements
------------

- Ansible 2+


Role Variables
--------------

### NTP Servers

NTP servers are **prefer**red servers. They should be set to your networks
internal NTP servers.

```yml
ntp_servers:
  - ntp1.domain.org
  - ntp2.domain.org
  - ntp3.domain.org
```

NTP fallback servers should only be set for the above-mentioned NTP servers.
Use regional pools.

```yml
ntp_fallback_servers:
  - 0.europe.pool.ntp.org
  - 1.europe.pool.ntp.org
  - 2.europe.pool.ntp.org
  - 3.europe.pool.ntp.org
```

If neither NTP servers nor fallback servers are specified, the default servers
in the distributed configuration are used.

NTP peers are defined like this:

```yml
ntp_peers:
  - ntp2.domain.org
  - ntp3.domain.org
```

Peers will get an automatic `restrict`ion, i.e. `nomodify notrap`.

### Restrict

Optionally ease restriction on the local network, i.e. `nomodify notrap
nopeer`.

```yml
ntp_local_network_restriction:
  net: 192.168.1.0
  mask: 255.255.255.0
```


Dependencies
------------

None.


Example Playbook
----------------

Add to `requirements.yml`:

```yml
---

- src: idiv-biodiversity.ntp

...
```

Download:

```console
$ ansible-galaxy install -r requirements.yml
```

### Top-Level Playbook

Write a top-level playbook:

```yml
---

- name: head server
  hosts: head

  roles:
    - role: idiv-biodiversity.ntp
      tags:
        - ntp

...
```

### Role Dependency

Define the role dependency in `meta/main.yml`:

```yml
---

dependencies:

  - role: idiv-biodiversity.ntp
    tags:
      - ntp

...
```


License
-------

MIT


Author Information
------------------

This role was created in 2017 by [Christian Krause][author] aka [wookietreiber
at GitHub][wookietreiber], HPC cluster systems administrator at the [German
Centre for Integrative Biodiversity Research (iDiv)][idiv].


[author]: https://www.idiv.de/groups_and_people/employees/details/eshow/krause-christian.html
[idiv]: https://www.idiv.de/
[wookietreiber]: https://github.com/wookietreiber
[ntp]: http://www.ntp.org/
