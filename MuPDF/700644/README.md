jpx_read_image contains a null deref when parsing a malicious or otherwise malformed jp2 file

AddressSanitizer:DEADLYSIGNAL
=================================================================
==31453==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x55f2e80a63bb bp 0x7ffd763d3530 sp 0x7ffd763d12c0 T0)
==31453==The signal is caused by a READ memory access.
==31453==Hint: address points to the zero page.
    #0 0x55f2e80a63ba in jpx_read_image source/fitz/load-jpx.c:857
    #1 0x55f2e80a6cb8 in fz_load_jpx source/fitz/load-jpx.c:909
