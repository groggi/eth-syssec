# Part 3: Security of x86-based Systems
## Application Security
- Requirements
	- Launch-time Integrity: correct application was started/loaded
	- Run-time Isolation: no interference from malicious software/peripherals possible
	- Secure Persistent Storage
	- Code should have launch-time integrity and run-time isolation
	- Volatile data should have launch-time integrity and run-time isolation
	- Persistent data should be able to use a secure persistent storage
- Required functionality
	- Launch-time Integrity: integrity (e.g. hashes) verification of initial code and data
	- Run-time Isolation:
		- prevent unauthorised modification of code and data
		- prevent run-time attacks to modify execution flow
	- Secure Storage:
		- confidentiality
		- integrity protection
	- This security functionality must be secured too
	- Unclear where and how to implement the functionality

## Platform Overview
- Processor: contains one or more CPUs (called "cores" in this lecture)
	- MMU: Memory Management Unit
	- Programmable Interrupt Controller
	- Intel VMX: Virtual Machine Extensions
	- L1, L2 caches for each core
	- L3 cache shared by all cores
- Chipset: connects processor to memory and peripherals
- Peripherals: connected to the chipset through multiple bus-interfaces

## OS-Based Security
- Privilege Rings
	- Ring 0: Kernel
	- Ring 1: Device Drivers
	- Ring 2: Device Drivers
	- Ring 3: Applications
	- Lower number -> higher privileges
	- Nowadays only Ring 0 and Ring 3 are used
	- Device drivers part of the kernel now
	- Processor tracks Current Privilege Level (CPL) using two register bits
	- Used to limit access to privileged instructions and I/O ports
	- In the past used to protect kernel memory
- Paging-based Security: Security relevant data in page table entries
	- Supervisor bit: if set, page only accessible by ring 0 (isolates operating system from applications)
	- RW bits: allows to distinguish between read-only and writeable pages
	- Execution disable (ED) bit: if set, page isn't executable (e.g. prevents run-time code injection)
- DMA through IOMMU
	- Use IOMMU (Input/Output Memory Management Unit)
	- Controls DMA access to physical memory
	- Set up by operating system
	- Allows to make sure DMA access is not done to security relevant pages (page tables, kernel pages, etc.)

## TPM
... TODO ... lecture 5, slide 19ff