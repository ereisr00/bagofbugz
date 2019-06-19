Three bugs that cause Qemu to crash due to improper input handling in Qemu TCG. 

These bugs could be used to crash a running emulated system (for example, a linux or windows OS) or any other program that uses the TCG. 
It is not required to run an executable to crash Qemu - all it takes is to cause the translation of a malicious basic block.
