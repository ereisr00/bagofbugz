AddressSanitizer:DEADLYSIGNAL
=================================================================
==24710==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x5651e30c70b9 bp 0x7ffe2ce7ac90 sp 0x7ffe2ce7ac50 T0)
==24710==The signal is caused by a READ memory access.
==24710==Hint: address points to the zero page.
    #0 0x5651e30c70b8 in tiff_paste_tile source/fitz/load-tiff.c:430
    #1 0x5651e30c8d8a in tiff_decode_tiles source/fitz/load-tiff.c:636
    #2 0x5651e30ce62e in tiff_decode_samples source/fitz/load-tiff.c:1308
    #3 0x5651e30cf306 in fz_load_tiff_subimage source/fitz/load-tiff.c:1391
    #4 0x5651e30cfb97 in fz_load_tiff source/fitz/load-tiff.c:1431

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV source/fitz/load-tiff.c:430 in tiff_paste_tile
