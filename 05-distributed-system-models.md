# Distributed System Models

## Reliable cloud application

1. Identify workloads and their usage requirements

2. Identify critical components and paths

3. Establish availability metrics

- Mean time to recovery (MTTR) and mean time between failures (MTBF)
  - Determine when to add redundancy
  - Determine SLAs

4. Define the availability (=uptime) targets: SLO and SLA

5. Failure mode analysis

6. Redundancy plan

7. Design scalability and load balancing

8. Implement Resiliency strategy

9. Manage the data: store, backup, recovery plan and replicate

10. Monitor and fault recovery

## Fault Tolerance

- Failure: system as a whole is not working

- Fault: some part of the system is not working

  - Node fault: crash(-stop/-recovery), deviate from algo
  - Network fault: dropped/delayed messages

- Fault tolerance: system as a whole continues working despite faults (a max. #fault assumed)

- Single point of failure (SPOF): node/network link whose fault leads to a failure

### Failure detectors

### Task: build a reliable system from unreliable components

## Models of distributed systems

### System models
