autoscale: true
build-lists: true
slide-dividers: #, ##
footer: inquiries@jonathan.industries
slidenumbers: true

# [fit] Distributed Machine Learning with Ray

Scaling AI Applications in Python with the Ray Framework

## Learning Outcomes

* Basics of distributed machine learning
* Use cases that __Ray__ is especially well-suited to solve
* Differences between **Ray** vs. other frameworks
* Internals of **Ray** and its execution model
* Context of the larger **Ray** ecosystem.

## Getting the Materials

[https://sr.ht/~hyphaebeast/ray-live-training/](https://sr.ht/~hyphaebeast/ray-live-training/)

## [fit] Do you have any experience with machine learning?

## [fit] Have you used a distributed computing framework?

### (like Spark, Flink, Hadoop, etc.)

# Why Scale?

## A Brief History

> In terms of size [of transistors] you can see that we're approaching the size of atoms which is a fundamental barrier, but it'll be two or three generations before we get that far—but that's as far out as we've ever been able to see.
-- Gordon Moore (2006)

## A Brief History

> We have another 10 to 20 years before we reach a fundamental limit. By then they'll be able to make bigger chips and have transistor budgets in the billions.
-- Gordon Moore (2006)

## Big Data

* __Long Data__: Large # of instances/records/rows/etc. 
* __Wide Data__: High dimensionality

## Big Models

* __Large Parameter Spaces__: Models with large number of parameters to learn
* __Prediction Cascades__: Complex pipeline of multiple prediction tasks

## Small Latencies

* Real-time predictions (i.e. website ad serving)
* Prediction Cascades... again (i.e. self-driving cars)

# What Scale?

* Memory
* Computation

# Introduction to distributed systems

> Distributed programming is the art of solving the same problem that you can solve on a single computer using multiple computers.
-- Mikito Takada[^1]

[^1]: [Distributed systems: for fun and profit](http://book.mixu.net/distsys/single-page.html)

---

![](figures/figure.009.png)

---

![](figures/figure.011.png)

---

![](figures/figure.012.png)

## Streaming Algorithms

_we won't talk about these now...._ 

## Paralellism

* __Data Parallelism__: Split instances between machines
* __Model Paralellism__: Split parameters between machines
* __Task Paralellism__: Split algorithms between machines

---

![](figures/figure.015.png)

---

![](figures/figure.016.png)

---

![](figures/figure.017.png)

---

![](figures/figure.018.png)

---

![](figures/figure.019.png)


# Problem Solved?

* _Coordination_: what happens when and to whom? And how do you synchronize these events
* _Communication_: how do the different nodes in your system talk to one another? And how do you talk to your nodes? 
* _Fault Tolerance_: is your system robust to network and machine failures ([because they will happen](https://queue.acm.org/detail.cfm?id=2371297))?

# This is what Ray helps us with 👇

[.build-lists: false]

* _Coordination_: what happens when and to whom? And how do you synchronize these events
* _Communication_: how do the different nodes in your system talk to one another? And how do you talk to your nodes? 
* _Fault Tolerance_: is your system robust to network and machine failures ([because they will happen](https://queue.acm.org/detail.cfm?id=2371297))?

# CAP Theorem[^2]

_C_onsistency: all nodes see the same data at the same time

_A_vailability: every request receives a response about whether it succeeded or failed

_P_artition tolerance: the system continues to operate despite arbitrary partitioning due to network failures

<br>

[^2]: Choose two: except you can’t really sacrifice _P_!

## [fit] Break

# Why Ray?

* _Distributed computing framework_ meant for a new generation of AI applications. 
* Emerged from _RL applications_
* _More generic_ programming model than Spark/Flink/Hadoop/etc.
* More _fault tolerant_ than MPI

---

## `multiprocess` meets RPC....


##  The Ray programming model

* Tasks (remote functions)
* Actors (remote classes)

## Tasks

```python
@ray.remote
def index(url):
    return requests.get(url)

futures = [index.remote(site) for site in internet]
print(ray.get(futures))
```

## [fit] Live Code

## Actors

[.column]
```python
@ray.remote
class Child(object):
    def __init__(self):
        self.name = random.choice(baby_names)
        self.age = 1

    def grow(self):
        self.age += 1

    def greet(self):
        return (
            f'My name is {self.name} '
            f'and I am {self.age} years old'
        )
```

[.column]
```python
children = [Child.remote() for i in range(4)]
[c.grow.remote() for c in children]
futures = [c.greet.remote() for c in children]
print(ray.get(futures))
```

## [fit] Live Code

## [fit] Break

## Distributed Training Architectures

* [Parallelized Stochastic Gradient Descent](https://papers.nips.cc/paper/4006-parallelized-stochastic-gradient-descent) (2010)
* [Hogwild!](https://papers.nips.cc/paper/4390-hogwild-a-lock-free-approach-to-parallelizing-stochastic-gradient-descent) (2011)
* [Downpour SGD (DistBelief)](https://research.google/pubs/pub40565/) (2012)
* [AllReduce](https://arxiv.org/abs/1110.4198) (2014)
* [Parameter Server](https://www.usenix.org/conference/osdi14/technical-sessions/presentation/li_mu) (2014)

## Will paralellization help me?

---

![](figures/figure.022.png)

---

![](figures/figure.023.png)

---

![](figures/figure.024.png)

---

![](figures/figure.025.png)

## Moral?

## It doesn’t how many machines you throw at a problem... at some point it is just not going to run any faster

## Distributed training with the parameter server

![right fit](figures/figure.038.png)

## System rather than algorithm[^3]

![right fit](images/parameter.png)

[^3]: [Scaling Distributed Machine Learning with the Parameter Server](https://www.usenix.org/conference/osdi14/technical-sessions/presentation/li_mu)

##  Ray internals[^4]

![right fit](images/internals.png)

[^4]: [Ray: A Distributed Framework for Emerging AI Applications](https://arxiv.org/abs/1712.05889)

## [fit] Live Code

## Deploying ML

---

![](figures/figure.030.png)

---

## ML infrastructure is varied

* MLaaS (SageMaker, Google AutoML, etc.)
* Containers (Fargate, GKE, etc.)
* IaaS (Ec2, GCP, etc.)
* On premise/data center

![right fit](figures/figure.031.png)

---

![](figures/figure.032.png)

---

![](figures/figure.033.png)

---

![](figures/figure.037.png)

## [fit] Next Steps

## Learning More DistML

[.hide-footer]

![right](images/scaling_ml_book.png)

[Scaling up Machine Learning](https://www.cambridge.org/core/books/scaling-up-machine-learning/6F1331FB8D57B77E5052F0E6956E3FBD)

---

## Future Live Training

* [Bayesian Hyperparameter Optimization](https://learning.oreilly.com/live-training/courses/bayesian-hyperparameter-optimization/0636920403685/) (Sept. 10)

## Q & A

Materials, mailing list, questions, feedback 👇
[https://sr.ht/~hyphaebeast/ray-live-training/](https://sr.ht/~hyphaebeast/ray-live-training/)
