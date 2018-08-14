@title[Introduction]

## testing stuff
in, generally, c++

@fa[arrow-down]

+++

`Agenda`

* The Basics |
* In Practice |
* Containers |

---

## The Basics

@fa[arrow-down]

+++

Many different types of c++ testing libraries out there:

* Google Test |
* Boost Test |
* Catch2 (header only!) |
* et al. |

+++

`unit tests`

Testing indvidual logical modules, e.g. a function

```
bool add_if_even(int& a, int b) {
  if(b % 2 == 0) 
    return false;
  a += b;
  return true;
}
```

+++

Enter... Google Test

```
TEST(mytests, success) {
  int a = 0;
  EXPECT_EQ(add_if_even(a, 2), true);
  EXPECT_EQ(a, 2);
}


TEST(mytests, failure) {
  int a = 0;
  EXPECT_EQ(add_if_even(a, 5), false);
  EXPECT_EQ(a, 0);
}
```
* Does my code compile and link
* Test API semantics
* No side-effects from successive tests, unless intended

+++

`integration tests`

Testing multiple modules together, e.g. your app + a database

+++

`system testing (end-to-end)`

Testing all workflows from the highest abstraction, e.g. GUI login

+++

`coverage tests`

How much of my code have I tested? 

Writing in an interpreted language? You need this to proxy as a compiler for catching typos.

+++

`CI/CD`

* Continuous Integration |
* Continuous Delivery |
* Continuous Deployment |

---

## In Practice

Even large, successful, HFTs don't do proper testing (Optiver, IMC, Sun...)

@fa[arrow-down]

+++

`too much friction`

* code not well-suited for unit tests |
* dependency hell |
* time to market rationalization |
* #TestingNotFirstClassCitizen |

+++

`organize well & tend your garden`

```text
myproj
├── app
│   └── main.cpp
├── libs
│   ├── libA
│   │   ├── include  <-- public headers *only*
│   │   ├── src      <-- implementation
│   │   └── test     <-- all tests for libA
│   ├── libB
│   │   ├── include
│   │   ├── src
│   │   └── test
├── README.md
```
+++

`use your build framework`

* choose a reasonably unopinionated framework (`CMake` is a fine choice)
* get your build right; clean all to fix a build is not acceptable

+++

`avoid static initialization`

```c++
class Securities {
  Securities(string path);

public:
  static Securities& instance(string path = "") {
    static Securities i(path)
    return i;
  }
};
```
* adds initialization dependency |
* adds side-effects across tests |
* increases testing burden |

+++

`move high burdening functions up the stack`

e.g. a database query in your code is high burden

+++

`how do I drive this?`

for every logical module you write, ask yourself "how do I drive this module with unit tests?"


naturally avoids dependency injections that increase testing burdens

+++

`lack of CI/CD`

Proper CI/CD forces accountability for failed test cases or poor coverage tests

+++

`integration tests are even harder`

* how do I test my app that depends on some pre-trade risk server and a MySQL database?
* most of my complex logic lies in integration tests, and I can't write those, so what's the point of writing any unit tests?

---

## Containers

Like virtual machines, but more lightweight

@fa[arrow-down]

+++

`end the hairiness with...`

* managing your build and runtime dependencies |
* running your integration tests |
* other life problems |

+++

Define your environment as a `Dockerfile`:

```Dockerfile
FROM centos:centos7


RUN yum install -y epel-release gcc gcc-c++
RUN yum install -y make autoconf cmake cmake3 meson
```

@[1,1](Build your image from an existing `centos:centos7` image)
@[4-5](Run whatever commands need to add your dependencies)

+++

Example

+++

`IoE: images of everything`

* Development environment are images |
* Delivery artifacts are images |
* DinD (docker in docker) for CI/CD |
* Images stored in docker registry |

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

Example

+++

---

?
