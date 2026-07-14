# labMachineScheduler Ontology

The ontology represents a shared research lab environment in which computers are registered with concrete technical properties such as storage capacity, read/write speed, RAM size and frequency, and CPU/GPU specifications. Each computer can be classified by access configuration (web, SSH, physical presence, AnyDesk) and by purpose (server or desktop). The model also supports software and hardware composition, as well as scheduling and user roles.

## Package Summaries

### `computer`
Defines the core computer model and its software-related concepts:
- `Computer`: a laboratory machine with a name, internal identifier, storage, memory, CPU, and GPU characteristics.
- `AccessConfiguration`: an enumeration for how a computer can be accessed or reached.
- `ComputerPurpose`: an enumeration for the intended use of the computer in the lab.
- `OperatingSystemType`: represents the type of operating system.

### `user`
Models the laboratory users, their roles, and scheduling activity:
- `Person`: a human actor in the scheduling process.
- `LabMachineScheduleUser`: a role played by users of the scheduling platform.
- `Student` and `Professor`: user roles with different permissions.
- `Permanence`: a relator representing a period in which a user is present in the lab.
- `SchedulePurpose`: an enumeration for scheduling reasons such as use, maintenance, or cancellation.
- `Schedule`: a relator representing the allocation of a computer during a time interval.


## Purpose
This ontology supports the design of a lab machine scheduling system by modeling:
- Hardware as attributes
- Operating system as a Type,
- Access methods and machine purpose,
- User roles and their interactions with scheduling,
- Time-bound reservations and their intended purpose.

It provides a structured vocabulary for building systems that govern laboratory computer availability, access, and usage by students and professors.

## Constraints and Business Rules

The current Tonto model does not include explicit constraint syntax in the available project files, so these rules are documented here as business requirements rather than as directly encoded ontology constraints.

1. A user with the `Student` role cannot create schedules with the purpose `MAINTENANCE`.
2. A user with the `Professor` role may modify a `Student` schedule to the purpose `CANCELED`.
3. Once a schedule is changed to `CANCELED`, that change is final and cannot be altered.
4. A `begin` timestamp must not be later than the corresponding `end` timestamp on the same object.
5. For two distinct `Permanence` records belonging to the same user, their time intervals must not overlap.
6. For schedules on the same computer, overlapping intervals are permitted only when exactly one overlapping schedule is `USE` or `MAINTENANCE` and the remaining overlapping schedules are `CANCELED`.

These constraints should be enforced through application logic, validation routines, or a complementary constraint mechanism if the Tonto tooling supports one in the future.

