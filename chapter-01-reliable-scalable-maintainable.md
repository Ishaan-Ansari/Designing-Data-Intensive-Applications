# Chapter 1: Reliable, Scalable, and Maintainable Applications

## Introduction

This chapter provides a high-level intuition for building **Reliable**, **Scalable**, and **Maintainable** data systems.

---

## Reliability

A reliable system is one that:

- Performs the function that the user expected, appropriately.
- Tolerates mistakes made by users or the system in unexpected ways.
- Delivers optimal performance as per requirements (under expected load and data volume).
- Prevents unauthorized access to the application.

### Fault vs. Failure

| Term | Definition |
|------|-----------|
| **Fault** | Anything that can go wrong within a component of the system. |
| **Failure** | When the whole system stops providing the expected service. |

> To ensure reliability, faults are **deliberately induced** to test system resilience.

---

### Types of Faults

#### Hardware Faults

- Hard disk crashes, RAM becoming faulty, etc. (MTTF — Mean Time to Failure — ranges from 10 to 50 years).
- **Prevention:** Adding redundancy, setting up RAID servers.

#### Software Errors

- Bugs that usually lie dormant until triggered by an unusual set of circumstances.
- **Prevention:**
  - Carefully think through assumptions in the system.
  - Allow processes to crash and restart deliberately (fail-fast).
  - Monitor and log system behaviour.
  - Verify invariants (e.g. ensure number of incoming messages == number of outgoing messages).

- **Example:**
    - **Cascading failure**: One server's slowdown causes others to overload

**Best practices to follow**:
```python
# Example: Circuit breaker pattern to prevent cascading failures
import time
from enum import Enum

class CircuitState(Enum):
    CLOSED = "closed"  # Normal operation
    OPEN = "open"      # Failing, reject requests
    HALF_OPEN = "half_open"  # Testing if service recovered

class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = CircuitState.CLOSED

    def call(self, func, *args, **kwargs):
        if self.state == CircuitState.OPEN:
            if time.time() - self.last_failure_time > self.timeout:
                self.state = CircuitState.HALF_OPEN
            else:
                raise Exception("Circuit breaker is OPEN")

        try:
            result = func(*args, **kwargs)
            self.on_success()
            return result
        except Exception as e:
            self.on_failure()
            raise e

    def on_success(self):
        self.failure_count = 0
        self.state = CircuitState.CLOSED

    def on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()

        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN

# Usage
breaker = CircuitBreaker(failure_threshold=3, timeout=30)

def call_external_service():
    # Simulated external API call
    response = requests.get("https://api.example.com/data")
    return response.json()

try:
    data = breaker.call(call_external_service)
except Exception as e:
    print(f"Service unavailable: {e}")
```




#### Human Errors

- **Prevention:**
  - Minimise opportunity for errors through clear abstractions and well-designed APIs.
  - Decouple places where mistakes are likely 
    - provide a fully functional, non-production sandbox to experiment safely and identify breaking points.
  - Test thoroughly at all levels: unit, integration, and system.
  - Make it fast to roll back changes.
  - Implement clear monitoring, logging, and telemetry.

---

## Scalability

Scalability refers to a system's ability to cope with increased load.


#### Describing Load
- Load can be described with ```load parameters```
    - LP depends on system architecture
    - ***Example:** Twitter
        - Usually 12k requests/sec
    
<!-- Understand Twitter's case study -->

#### Describing performance
- You can look at it in two ways to test when you increase the load parameter:
    - While keeping the resources unchanged.
    - How much increase in resources are needed inorder to maintain the same performance.
- Metrics used -  ```Throughput``` (Number of records processed per second).

> NOTE: Latency - Duration that a request is waiting to be handled. \
> NOTE: Response time - is what client sees: it includes netwrok delays.

- Incase when we are measuring the response time we usually prefer ```percentiles``` over mean time.

- ```p50``` Half of the user request were served in less than median time.
- Inorder to figure out how bad your outliers are you can look at higher percentiles (```p50```, ```p95```, ```p999```)

- Percentials are often used in Service Level Objective (SLOs) or Service level Agreements(SLAs).

- ```head-of-line-blocking``` - Sometimes the tasks hold up to some prcessing of subsequent requests that causes delay 

> It is IMPORTANT to measure response time at client side as well.

#### Approaches for coping with load
- Distributing load across multiple machines is also
known as a ```shared nothing architecture```.
- Some systems are ````elastic```, meaning that they can automatically add computing resour
ces when they detect a load increase,

-While distributing stateless services across multiple machines is fairly straightfor
ward, taking stateful data systems from a single node to a distributed setup can intro
duce a lot of additional complexity.

> In an early-stage startup or an
unproven product it’s usually more important to be able to iterate quickly on product
features, than it is to scale to some hypothetical future load.

## Maintainabilty
-  the majority of the cost of software is not in its initial develop ment, but in its ongoing maintenance 
    - fixing bugs, keeping its systems operational
    - investigating failures
    - adapting it to new platforms
    - modifying it for new use cases
    - repaying technical debt, and adding new features.

we will pay particular attention to three design principles for software sys
tems

#### Operability
- Monitoring
- Tracking down cause of problems 
- Keeping software and platform up-to-date
- Maintaining security
- Preserving the organization's knowledge about the system
<!-- Write more technically dense, in your own language -->

#### Simplicity
- As the product get larger the software becomes more tightly coupled and challenging to comprehend
- This directly impacts to productivity of engineers who are working on it
- Making a system simpler does not necessarily mean reducing its functionality; it can also mean removing accidental complexity.
- ```Abstraction``` one of the best ways to remove complexity from code

#### Evolvability
-  ```Agile``` working patterns provide a framework for adapting to change
- ```Test-driven development (TDD)``` 
