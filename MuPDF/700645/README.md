Function xps_open_document receives a file name that represents a path to an xps document or directory.

The functions checks if it received a directory by searching for the path "/_rels/.rels". If the special path is included in the file path the function proceeds to copy the path into a buffer of size 2048. At this point the function assumes that "/_rels/.rels" is included in the first 2048 bytes of the filepath variable, and, therefore, also in the new, possibly truncated variable.

This assumption is false, and can result in a null deref when given a path in which the "/_rels/.rels" part is included after the first 2048 bytes.

=================================================================
==18998==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x55b5455b4172 bp 0x7ffc34822d10 sp 0x7ffc34822450 T0)
==18998==The signal is caused by a WRITE memory access.
==18998==Hint: address points to the zero page.
    #0 0x55b5455b4171 in xps_open_document source/xps/xps-zip.c:171
    #1 0x55b545461b35 in fz_open_document source/fitz/document.c:202

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV source/xps/xps-zip.c:171 in xps_open_document
==18998==ABORTING

to reproduce: 
1. create folders recursively until the total length of the innermost folder is longer than 2048.
2. create "_rels/.rels/" directories
3. open a file inside the innermost folder of the path
