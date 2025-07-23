Objective:
The objective of this project is to identify and analyze a suspicious Linux kernel module (spatch.ko) discovered in a controlled investigation environment. The goal is to determine its purpose, detect any embedded malicious behavior, and extract a concealed flag from within the module.

This project demonstrates practical techniques used in kernel-level rootkit detection, binary string analysis, and secure system auditing — essential skills in cybersecurity, threat analysis, and digital forensics.

Tools & Commands Used:
Task	Command	Purpose
List loaded kernel modules	lsmod	Identify non-standard or unauthorized modules
Display module metadata	/sbin/modinfo spatch	Obtain file path and attributes
Extract readable strings	strings /lib/modules/.../spatch.ko	Detect embedded indicators or secrets
Decode hex data	`echo HEX	xxd -r -p`

Execution & Output:

# 1. List loaded modules to identify suspicious entries
lsmod

# Output:
# spatch                 12288  0

# 2. Retrieve detailed information about the module
/sbin/modinfo spatch

# Output:
# filename: /lib/modules/6.8.0-1016-aws/kernel/drivers/misc/spatch.ko
# description: Cipher is always root
# author: Cipher

# 3. Inspect the binary content of the kernel module
strings /lib/modules/6.8.0-1016-aws/kernel/drivers/misc/spatch.ko | grep "secret"

# Output:
# 6[CIPHER BACKDOOR] Here's the secret: 54484d7b73757033725f736e33346b795f643030727d0a

# 4. Decode the embedded hex string to reveal the hidden flag
echo 54484d7b73757033725f736e33346b795f643030727d0a | xxd -r -p

# Output:
# THM{sup3r_sn34ky_d00r}
Results:
A kernel module named spatch was detected. It is not part of any known or trusted kernel distribution.

The module's metadata revealed custom author and description fields referencing a "Cipher" — a common tactic used to label malicious implants.

A hidden message embedded in the binary contained a hexadecimal-encoded flag.

The decoded result produced the string:

Extracted Flag:

THM{sup3r_sn3ky_d00r}
Conclusion:
The investigation successfully revealed a stealthy backdoor implemented as a loadable kernel module. The module was small in size (12 KB), self-contained, and unaffiliated with any other components — all typical traits of a rootkit.
