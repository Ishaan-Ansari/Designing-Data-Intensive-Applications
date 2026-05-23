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
    

- 

#### Describing performance



#### Approaches for coping with load




