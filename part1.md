# Part 1

## Side Channel Attack
### Power Analysis Attack (Power Cryptanalysis Attack)
- Attack allows to retrieve a secret
- Trace power consumption of device
- Trace: Measurement of consumption during a single execution
	- Depends on the secret and input
- Simple Power Analysis (SPA) uses a single trace
- Differential Power Analysis (DPA):
	- Statistical analysis of multiple traces
	- similar to timing cryptanalysis
	- Assumption: power consumption depends not only on instruction but also the value of the operands
		- storing 1 vs. 0 in a register/memory
		- shifts and rotations depend on number of positions
		- logical and arithmetic
	- ... TODO ...
- High-Order Differential Power Analysis uses complex statistical methods, again multiple traces
- Primary Targets: Smartcards, RFID chips, Sensor nodes, etc. Basically embedded devices
- Physical access to device needed

#### Technical Details
- Power describes the consumed energy per unit time
- Dynamic power consumption
	- Power to charge transistor gate
	- State changes
	- Discharge (e.g. from 1 to 0) is free => if state changes \\(f\\) times only \\(\frac{f}{2}\\) times a charge happens which adds up to the dynamic power consumption.
- Static power consumption
	- Power consumed while no states changes happen (i.e. no gates need charging)
- CMOS gates
	- \\(0 \to 0, 1 \to 1\\) nearly no power needed
	- \\(0 \to 1, 1 \to 0\\) high power consumption
- While measuring use average to reduce noise

#### Practice
- Create trace containing instructions depending on the key
- Interesting instruction sets
	- rounds in DES
	- square vs. multiply in RSA exponentiation (see exercise)
	- hamming weights of words

#### SPA on RSA (Exercise)
- Attack on top-down square and multiply exponentiation
	- done during signing and decryption
- Observation
	- Squaring followed by squaring \\(\Rightarrow\\) bit of the exponent is zero
	- Squaring followed by multiplication \\(\Rightarrow\\) bit of the exponent is one
- On slides: Square has higher power consumption than multiplication
- In Exercise: ... TODO ...

#### Protections
- Desynchronization: change power consumption randomly
	- e.g. insert dummy instructions
	- Dummy instructions can be recognised by SPA
- Noise generator: piece of hardware to randomise consumption
	- Using larger set of traces can counter this. Noise can be filtered out.
- Filter at power input & physical shielding
	- In case of physical access, can be removed/deactivated
- Software balancing
	- Results in significant overhead to the computation (not very practical)
- Hardware balancing
	- Processor needs the same amount of power for all instructions, operand's values
	- Costly and hard to design
- TODO: Shamir's countermeasure

### Side Channel Attack: Cache-timing Attack on AES
- Differential attack
- ... TODO ... read paper
- Can recover the secret key by measuring the time
- Attacker sends specific plaintext (known-plaintext attack) to server and receives the ciphertext. Measures the time the server needs.
- Attack possible due to AES Design: Very hard to write constant time high performance AES software for general-purpose computers.
- Vulnerability exists because implementations *use lookup tables* for internal operations, such as S-Boxes.
	- Time to access a specific table depends on the address of the position
	- Data will be in cache or in memory, different access times
		- L1 and L2 caches, RAM, Disk
	- Attack can learn about the address being accessed by looking at the time needed for access
- In AES table lookup, i.e. memory addresses, are derived from the secret key and plaintext
- ... TODO ... after reading paper slides 41ff
- Lessons learned
	- General: write constant time security software
	- Cache-timing attacks countermeasures:
		- avoid memory access
		- alternative lookup tables
		- data-oblivious memory access patterns
		- application specific masking
		- static or disabled caches
		- dynamic table storage
		- hiding the timing
	- Most countermeasures are expensive in regard to money (e.g. redesign cipher, redesign hardware) or in regard to performance

## Tamper Resilience


## Smartcards


## API Attacks
