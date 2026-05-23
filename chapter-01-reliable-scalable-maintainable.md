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
