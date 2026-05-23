# Chapter 1: Reliable, Scalable, and Maintainable Applications

## Introduction
This chapter helps in giving an high level intuition for building ```'Reliable'```, ```'Scalable'``` and ```'Maintainable'``` data systems.

### Reliabilty

- Application performs appropritely the function that user expected.
- Tolerance incase user or system makes any mistake in unexpected ways.
- Performance is is optimal as per requirements (Under expected load and data volume).
- Prevents any unauthorized access to application

### Fault vs Failure
- Things which can go wrong is termed as ```Fault```
- When the whole system stops providing expected service then it is considered as ```Failure```
- To ensure reliabilty of the system, we deliberatly induce faults.

    - ##### Hardware Faults
        - Hard disk crash, RAM becoms faulty (MTTF - Mean time to failure - 10 to 50 years)
        - Prevention - Adding redudancy, setting up RAID server

    - ##### Software errors
        - Bugs in the code that usually lie dormant until they're triggered by some unusual set of cirumstances
        - Prevention
            - Carefully thinking about assumptions
            - Allowing process to crash deliberately 
            - Monitoring and logs 
            - Incase some guarantee is provided (ensure no. of incoming == no. of outgoing requests)

    - ##### Human errors
        - Prevention
            - Minimize opportuinity for errors 
                - Clear abstraction, APIs




### Scalabilty


