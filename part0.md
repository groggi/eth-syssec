# Part 0

## Basics
- Stream Cipher: processes a message bit (byte) by bit (byte)
- Block Cipher: encrypt/decrypt blocks of a message
- Message Authentication Code (MAC)
	- Input: message, secret key
	- Output: authenticator of fixed length (the MAC)
	- Message Authentication: \\(\operatorname{MAC} = C(k, m)\\)
	- Message Authentication & Confidentiality: \\(\operatorname{Enc}(k_2, (m \, || \, C(k_1, m)))\\)

## Block Ciphers
- Encrypt/Decrypt blocks of \\(N\\) bits
- Each block interpreted as a symbol of an alphabet of size \\(2^N\\)
- Converts such a symbol (i.e. a block) plaintext to a ciphertext symbol. \\(2^N!\\) such mappings exists
- Secret key used to select one of the mappings
- Ideal Block Cipher:
	- Use any of the \\(2^N!\\) possible mappings
	- Would require a secret of \\(\log(2^N!)\\) bits
	- Such secrets are infeasible (e.g. \\(N = 64 \Rightarrow \, \approx 10^{11} \text{GB}\\))

### Practical Block Ciphers
- Use \\(K\\) bits to select a random subset of \\(2^K\\) mappings
- Important selected mapping is **random**, then we get a good approximation of the ideal block cipher
- Use Shannon's confusion and diffusion principles:
	- Confusion: Each bit of the ciphertext should depend on the whole secret. If one bit changes in the key the entire ciphertext should change.
		- Use small Pseudorandom Permutations (PRPs) \\(f_i(k, m_i)\\) where \\(f_i\\) depends on the secret \\(k\\) and is often called a Substitution-Box (S-Box)
		- Each \\(f_i\\) returns \\(b = |f_i|\\) bits
		- Confusion done by \\(F_k(m) = F_k(m_1 m_2 \cdots m_n) = f_1(k, m_1) f_2(k, m_2) \cdots f_n(k, m_n)\\)
		- Because splitting the message \\(m\\) up and put each piece into one of the \\(f_i\\) we get only \\(\leq b\\) bits changed in \\(F_k\\) if one bit changes in \\(m\\)
	- Diffusion: ciphertext bits should depend on plaintext bits in complex way. If bit \\(i\\) changes in plaintext, bit \\(j\\) in ciphertext should change with probability \\(p=\frac{1}{2}\\).
		- Takes the output of \\(F_k\\) by re-odering the bits
		- Permutation can be independent of the secret
	- Implemented using Feistel Networks (e.g. in DES), Substitution-Permutation Networks (e.g. in AES)
	- Run Confusion and Diffusion multiple times (i.e. multiple rounds)
	- Overall security depends on the key schedule, S-Boxes and how the permutations are done
	- Use of avalanche effect
		- small changes to input impact whole output
		- S-Boxes designed in a way that one bit changing in the input results in at least two bits changed in the output of the S-Box
		- Permutations designed in a way that output of one S-Box is the input of multiple S-Boxes in the next round