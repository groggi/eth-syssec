# Part 4: Trustworthy Computing and Attestation
## Basic Approaches
- Metric to evaluate approaches done by the size of the Trusted Computing Base (TCB): The amount of components one has to trust
- Put program code into ROM (read-only)
	- simple to do
	- attacker can't inject code
	- software can't be updated easily
	- control flow based attacks still possible
	- whole system is in TCB
	- impractical nowadays for most systems because of downsides
- Use Secure Boot
	- only load code with valid signatures
	- valid signature doesn't mean code has no security relevant problems
	- attacker needs only to compromise one component
	- whole system is in TCB
	- updates hard to verify
	- only small improvement over ROM solution, still weak security guarantees
- Use Virtual Machines for isolation
	- Isolate application by running each one in its own virtual machine
	- TCB is smaller: Hardware, Hypervisor / Virtual Machine Manager, operating system and application inside the isolated environment
	- However, VMM is still a large and complex part of the TCB
	- Complex to set up, not suitable for common day users

## Attestation for Software Integrity
- Possible to verify the correct software is running on a potentially untrusted device
- Gives code integrity, a powerful property
- Checks against a database to verify
- General Approach:
	1. Isolate execution environment from untrusted components
	2. Externally validate execution environment
	3. After trust established, autonomous operation of execution environment
- Core Mechanisms
	- Isolated execution trough hardware ensuring partition
	- Remote attestation for external validation
	- Sealed storage to enable secure execution after local root of trust established
- Adversary Model
	- Remote adversary: can compromise operating system, application and may control network communication
	- Minimal local physical attack capabilities: reboot, install malicious software, deploy malicious (USB) device

## TPM
see [TPM](tpm.md).

## Acronyms
- TCG: Trusted Computing Group
- TPM: Trusted Platform Module
- PCR: Platform Configuration Register
- TCB: Trusted Computing Base
- SRTM: Static Root of Trust for Measurement
- DRTM: Dynamic Root of Trust for Measurement
- SLB: Secure Loader Block
- TXT: Trusted Execution Technology

