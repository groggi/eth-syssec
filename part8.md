# Part 8: Securing Commercial OSs & SELinux
- Basic idea: take common operating system (e.g. Linux) and retrofit it to be secure
- Should include at least
	- Reference Monitor: Monitors and enforces security policy
	- Complete Mediation: All sensitive operations are handled by Reference Monitor
	- Tamperproof: Security mechanisms can't be modified by untrusted processes
	- Verifiable: should be small enough to audit / prove it satisfies goals

## Secure Linux
- Generic Reference Monitor inside kernel
- Different security models can be used by just loading specific kernel module
- Linux Security Module (LSM) Framework:
	- Hook security related functions and data structures
	- Allows to register and initialize security modules (e.g. AppArmor, GRSec, SELinux)
	- Some security attributes available to User Space through `/proc`
	- Very small performance overhead, especially if no security module loaded
	- Over 150 hooks defined

### SELinux
- Mandatory access control for Linux
- Done by using LSM
- Converts input to LSM hooks into authorization queries
- Contains Policy Store that is queried
- Files must be labeled on disk
- Default set to *deny*
- Possible to confine user by adding user to `user_u`. Such a user can't use application with `setuid` bit set or execute files inside their home directory
- Employs separation, one process can't be used as an attack vector against another process
- Modes
	- Disabled: not running, only Linux' permissions used
	- Permissive: Policy not enforced, queries resulting in a denial are logged
	- Enforcing: Policy is enforced, granting/denying based on rules
- Policies
	- Strict: everything not covered by policy is denied, programs may fail at runtime because of too strict rules
	- Targeted: strong policy for specific applications, all other applications run without special restrictions
	- Write policies by starting from a template/reference policy, then run SELinux in permissive mode to log denials and modify policy accordingly
