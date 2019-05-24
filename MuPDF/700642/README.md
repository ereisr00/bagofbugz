
AddressSanitizer:DEADLYSIGNAL
=================================================================
==25690==ERROR: AddressSanitizer: FPE on unknown address 0x56250256ca26 (pc 0x56250256ca26 bp 0x7ffd983d1930 sp 0x7ffd983d1870 T0)
    #0 0x56250256ca25 in tiff_decode_ifd source/fitz/load-tiff.c:1261
    #1 0x56250256e2ed in fz_load_tiff_subimage source/fitz/load-tiff.c:1390
    #2 0x56250256eb97 in fz_load_tiff source/fitz/load-tiff.c:1431


AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: FPE source/fitz/load-tiff.c:1261 in tiff_decode_ifd
==25690==ABORTING
