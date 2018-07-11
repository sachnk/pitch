@title[Introduction]

## exchange tech & stuff

@fa[arrow-down]

+++

`Agenda`

* Meat & Potatos |
* Containers |
* Architecture |

---

## Meat & Potatos

@fa[arrow-down]

+++

`centralize user mgmt`

<i>JumpCloud</i> is a <i>DaaS</i>

* LDAP |
* Single sign-on |
* Resource control /w 2FA |

+++

`everything in the cloud`

Use Docker containers, or AWS CloudFormation templates

* Source control |
* Issue tracking|
* Wiki |
* CI/CD |
* Docker registry |

+++

`embrace your inner network engineer`

* Segment development, staging, and production cloud resources into separate networks|
* Use distinct VPCs and corresponding VPN servers for each network|
* No direct connection to any resources directly from the internet (e.g. no ssh over internet)|
* Use Route 53 to perform DNS on internal resources|

---

## Containers

Like virtual machines, but more lightweight

@fa[arrow-down]

+++

Dockerfile

```
FROM centos:centos7

RUN yum install -y gcc gcc-c++


RUN cd /tmp && wget https://dl.bintray.com/boostorg/release/1.66.0/source/boost_1_66_0.tar.gz \
  && tar xfz boost_1_66_0.tar.gz && rm boost_1_66_0.tar.gz && cd boost_1_66_0 \
  && ./bootstrap.sh --prefix=/usr/local \
  && ./b2 install
```

@[1,2](Build your image from an existing `centos:centos7` image)
@[3-9](Run whatever commands need to add your dependencies)

+++

`images of everything`

* Development environment are images |
* Delivery artifacts are images |
* DinD (docker in docker) for CI/CD |
* Images stored in private docker registry |

+++

`Orchestration`

Google purpotedly runs 1 billion containers a week...
<br><br>
`¯\_(ツ)_/¯`

+++

`docker-compose`

Orchestrate your containers locally

* Launch multiple containers as services |
* Define volumes |
* Define networks |
* Upgrade your unit tests to integration tests |

+++

`docker swarm`

Orchestrate your containers across multiple servers

* Superset of docker-compose |
* Replicas |
* Self-healing |
* Mesh network |
* Kubernetes competition |

---

## Architecture

Separation of concerns, messaging, persistance, etc

+++

`apps`

* cross, c++ matching engine|
* risk, c++ risk layer|
* spool, c++ database upserter|
* gateway, node.js REST API|
* dataplant, node.js WebSocket API|
* painter, react.js website|
* silk, python blockchain mgmt|

+++

`messaging`

* raw sockets |
* zmq |
* nanomsg |
* rabbitmq ✔ |
* kafka ✔ |

+++

`event sourcing w/ kafka`

* single source of truth |
* immutable event log |
* checkpoints |
* recovery |
* exactly-once semantics |

+++

`kafka example`

```
0| ACCEPTED    B 100 @ 1.00
1| ACCEPTED    S 200 @ 2.00
2| ACCEPTED    S 300 @ 3.00
3| CANCELED    2
4| CHECKPOINT
               B 100 @ 1.00
               S 200 @ 2.00
5| CANCELED    1
6| CHECKPOINT
               B 100 @ 1.00
```

+++

`better than snapshots`

* prevents "snapshot-storms" from downstream apps
* store event offsets in database, e.g. calculated positions as-of a particular event offset

+++

`database changes w/ rabbitmq`

use stored procedure to capture changes and do a <i>guaranteed</i> notify to interested parties

---

?
