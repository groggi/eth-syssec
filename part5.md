# Part 5: Software-Based Attestation
- Achieve dynamic root of trust without hardware support
- Again we'd like to externally verify code execution
- Reasons for using software-based attestation
	- devices without hardware support
	- software-based attestation doesn't need a secret
	- allows additional use cases
- Initial situation:
	- Device \\(D\\) on which application runs is untrusted
	- External verifier \\(V\\) is trusted
	- Verifier wants to obtain proof about the memory content and integrity of the device
	- Device runs a verification function to create a proof
	- Verifier sends challenge and checks response

## Strawman Verification Function:
- Approach 1
	- Verifier asks device to use cryptographic hash function over memory
		1. \\(V \to D: \text{request checksum}\\)
		2. \\(D \to V: \operatorname{SHA-3}(\text{Memory})\\)
	- Attack on scheme: malicious code precomputes correct hash and replays it the moment requested
- Approach 2
	- Verifier picks a random challenge
	- Device calculates a MAC (Message Authentication Code) using the verifier's challenge as the key
		1. \\(V \to D: \text{request checksum, send random key along}\\)
		2. \\(D \to V: \operatorname{HMAC-SHA-3}(K, \text{Memory})\\)
	- Attack on scheme: Precompute correct checksum over expected memory and replay
## Reflection
- Software verifies its own operation
- Relies on predicting and monitoring processor's behaviour
- Memory is filled with random data (high entropy)
- Verification function
	- fills memory with random data
	- initiates clear system state
	- disables all interrupts
	- computes hash over whole memory
	- returns hash and system state
- Verifier checks time it took to calculate answer, checks returned hash and system state

## Genuinity
- Verifier sends code for checksum calculation to device
- Verification function: randomize memory content to cause unpredictable cache misses, integrates hardware parameters into checksum
- Simulation of verification function much slower because of complexity to simulate architectural features (e.g. cache misses)

## SWATT
- **S**oft**w**are based **att**estation for embedded devices
- Verifier sends nonce to device
- Verification function: pseudorandom memory traversal for memory checksum computation
- Verifier records time to calculate checksum and verifies checksum
- Malicious code would have to verify each memory access to replace memory reads to changed memory location with expected content. Results in a detectible time overhead
- Verification function done in a way that results in additional computation (with time impact) if tampering occurs
- Does not scale well for large memory
- Memory may contain secrets or dynamic data
- Verification function can be modified to check only small memory ares, must include checksum function itself. However, introduces new attack vectors
	- attacker computes checksum over a copy of the memory
	- improve verification function to include program counter and data pointer. Attacker needs more time for simulation, can be detected

## Pioneer
- Verifies integrity and guarantees execution of code on legacy platforms
- Challenges on x86
	- Execution time non-deterministic (out-of-order execution, cache and virtual memory, thermal effects)
	- Complex instruction set and architecture, how to ensure code is optimal? (we measure time)
	- DMA based attacks
	- Interrupt based attacks
	- Attacks using exceptions
	- Virtualization based attacks
- Approach
	1. Verify code integrity using SWORT
	2. Set up untampered code execution environment
	3. Execute code
- Protocol
	1. \\(V \to D: \text{nonce, input}\\)
	2. \\(D \to V: \text{checksum}\\)
	3. \\(D \to V: \text{hash}\\)
	4. \\(D \to V: \text{output}\\)
	5. Verifier checks if time between first and second step is smaller than the expected time
	6. Verifier checks it checksum is equal to the expected one
- Intuition for correctness: Checksum is incorrect or checksum is computation is slowed down by attacker
	-  If verification function is modified, additional time is spent
	-  Fakes creation of untampered code execution environment
	-  Potential attacks
	-  Execution tampering attacks by running malicious OS/VMM at high privilege level and getting control through interrupts/exceptions
	-  Checksum forgery attacks
		-  Memory-copy
		-  Data substitution
		-  Code optimization
		-  Parallel execution
		-  Exploiting superscalar architecture
		-  Precomputation/replay attacks

## ICE Key Establishment
- Use Indisputable Code Execution (ICE) to compute a checksum faster than any other node in a sensor network
- Use this checksum as a short living shared secret
- Protocol can prevent MitM attacks without authentic information or a shared secret beforehand
- Attacker can know entire memory before key establishment of both parties
- Attack must introduce a more powerful node into network, no remote attack possible
