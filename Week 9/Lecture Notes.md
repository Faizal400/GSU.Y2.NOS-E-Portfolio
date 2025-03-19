# Week 9 Virtualisation and Cloud

## Introduction to Virtualization
Virtualization is the creation of a virtual (rather than actual) version of something, such as an operating system, a server, a storage device, or network resources.

### Definitions
- **Virtual Machine (VM)**:
> - "A virtual machine is an abstraction of a complete compute environment through the combined virtualization of the processor, memory, and I/O components of a computer."
  - VMs emulate physical hardware, allowing multiple operating systems (guests) to run on a single physical machine (host).
  - Each VM has its own OS, applications, and resources.

- **Hypervisor**:
> - "The hypervisor is a specialized piece of system software that manages and runs virtual machines."
  - The hypervisor (also known as a Virtual Machine Manager or VMM) creates and manages VMs, allocating resources and isolating them from each other.
  - There are two main types of hypervisors:
    - Type 1 (Bare-metal): Runs directly on the hardware, like VMware ESXi or Xen.
    - Type 2 (Hosted): Runs on top of an existing OS, like VMware Workstation or VirtualBox.

### Virtualization Implementations
The three basic implementation techniques of virtualization are:
