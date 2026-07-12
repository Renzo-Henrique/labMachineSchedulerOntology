# labMachineScheduler Ontology

This ontology models a laboratory machine scheduling platform focused on managing lab computers, their hardware and software components, and the users that reserve them. It captures the relationships between machines, access methods, computer types, and user schedules to support a system where professors and students interact with laboratory resources.

## General Description
The ontology represents a shared research lab environment in which computers can be registered as desktops or servers and accessed through multiple methods such as web interfaces, SSH, physical presence, or AnyDesk. It also distinguishes user roles and the scheduling of machines for use or maintenance, providing a structure for managing availability and reservation intent.

## Package Summaries

### `computer`
Defines the core `Computer` kind and the access/type classifiers used by machines:
- `Computer`: represents a laboratory machine with a name and associated access/type classifications.
- `TypeOfAcess`: a mode capturing the different access methods available for a computer.
- `WebAcessType`, `SshAcessType`, `PhysicalAcessType`, `AnydeskAcessType`: specialized access methods for remote or local use.
- `typeOfComputer`: a mode for classifying computers by equipment type.
- `ServerComputerType`, `DesktopComputerType`: specialized categories for lab servers and desktop workstations.
- Relations connect each computer to one or more access methods and to a single computer type.

### `computerComponents`
Describes the hardware and software components that compose a computer:
- `Software`: general software artifacts for lab machines.
- `OperatingSystem`: the installed system software on a computer.
- `Hardware`: the physical parts that make up a computer.
- `StorageUnit`, `RandomAcessMemory`, `CentralProcessingUnit`, `GraphicsProcessingUnit`: core hardware components used by a computer.
- Component relations express that a computer is composed of an operating system and one or more storage, memory, CPU, and GPU components.

### `user`
Models the laboratory users, their roles, and scheduling activity:
- `Person`: a human involved in the lab scheduling system.
- `LabMachineScheduleUser`: a user of the platform, with contact information.
- `Student`: a user role representing a student reserving machines.
- `Professor`: a user role representing a professor with additional management capabilities.
- `Permanence`: a relator capturing periods of laboratory presence for a user.
- `Schedule`: a relator representing a reserved interval for a computer and the purpose of that reservation.
- `SchedulePurpose`: an enumeration supporting reservation intents such as use, maintenance, or cancellation.
- Mediation relations connect users to their permanence records, schedules, and the computers those schedules reference.

## Purpose
This ontology supports the design of a lab machine scheduling system by modeling:
- machine inventories and their hardware/software components,
- access methods and machine types,
- user roles and their interactions with the scheduling process,
- time-bound reservations and their intended purpose.

It provides a structured vocabulary for building systems that govern laboratory computer availability, access, and usage by students and professors.

## Constraints and Business Rules

The current Tonto model does not include explicit constraint syntax in the available project files, so these rules are documented here as business requirements rather than as directly encoded ontology constraints.

1. A user with the `Student` role cannot create schedules with the purpose `MAINTENANCE`.
2. A user with the `Professor` role may modify a `Student` schedule to the purpose `CANCELED`.
3. Once a schedule is changed to `CANCELED`, that change is final and cannot be altered.
4. A `begin` timestamp must not be later than the corresponding `end` timestamp on the same object.
5. For two distinct `Permanence` records belonging to the same user, their time intervals must not overlap.
6. For schedules on the same computer, overlapping intervals are permitted only when exactly one overlapping schedule is `USE` or `MAINTENCE` and the remaining overlapping schedules are `CANCELED`.

These constraints should be enforced through application logic, validation routines, or a complementary constraint mechanism if the Tonto tooling supports one in the future.

