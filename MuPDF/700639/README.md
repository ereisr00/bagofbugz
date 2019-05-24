Out of bounds read/write in fz_unpack_tile due to insufficient bound checks

AddressSanitizer:DEADLYSIGNAL
=================================================================
==11801==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x5596fa69a871 bp 0x7ffe797b7980 sp 0x7ffe797b7910 T0)
==11801==The signal is caused by a READ memory access.
==11801==Hint: address points to the zero page.
    #0 0x5596fa69a870 in fz_unpack_tile source/fitz/draw-unpack.c:172
    #1 0x5596fa6225f4 in fz_load_tiff_subimage source/fitz/load-tiff.c:1399
    #2 0x5596fa622789 in fz_load_tiff source/fitz/load-tiff.c:1431

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV source/fitz/draw-unpack.c:172 in fz_unpack_tile
==11801==ABORTING
