@title[Introduction]

## exchange tech & stuff

##### a demystification of sorts...

@fa[arrow-down]

+++
@title[Piecemeal Lists]

* Meat & Potatos |
* Containers |
* Architecture |

---

@title[Purpose]

## Meat & Potatos

i.e. environments

@fa[arrow-down]

+++

@title[Piecemeal Concepts]

Decide everything lives in AWS (or GCP, et al)

+++

@title[Piecemeal Lists]

Prefer on-premise over hosted solutions; use Docker containers, or AWS CloudFormation templates

* Source control |
* Issue tracking|
* Wiki |
* CI/CD |
* Docker registry |

+++

@title[Piecemeal Concepts]

Define company users and control access to resources

+++

@title[Piecemeal Lists]

<i>JumpCloud</i> is a <i>DaaS</i>

* Centralized user management |
* 2FA |
* Single sign-on |
* Resource control |

+++

@title[Piecemeal Concepts]

Partition cloud resources into separate networks

+++

@title[Piecemeal Lists]

Require VPN connections... no ssh over internet!

* Development |
* Staging |
* Production |

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

@title[Piecemeal Lists]

`Images for Everything`

* Development environment are images |
* Delivery artifacts are images |
* DinD (docker in docker) for CI/CD |

+++

`Orchestration`

Google purpotedly runs 1 billion containers a week...
<br><br>
`¯\_(ツ)_/¯`

+++

@title[Piecemeal Lists]

`docker-compose`

Orchestrate your containers locally

* Launch multiple containers as services |
* Define volumes |
* Define networks |
* Upgrade your unit tests to integration tests |

+++

@title[Piecemeal Lists]

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

@title[Piecemeal Lists]

`apps`

* cross, c++ matching engine|
* risk, c++ risk layer|
* spool, c++ database upserter|
* gateway, node.js REST API|
* dataplant, node.js WebSocket API|
* painter, react.js website|
* silk, python blockchain mgmt|

+++

@title[Piecemeal Lists]

`messaging`

* raw sockets |
* zmq |
* nanomsg |
* rabbitmq ✔ |
* kafka ✔ |

+++

@title[Piecemeal Lists]

`event sourcing w/ kafka`

* single source of truth |
* immutable event log |
* checkpoints |
* recovery |
* exactly-once semantics |

+++

@title[Piecemeal Lists]

`database changes w/ rabbitmq`

use stored procedure to capture changes and do a <i>guaranteed</i> notify to interested parties

---

?
