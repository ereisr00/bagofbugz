
tiff_unpredict_line contains a stack based buffer overflow
function signature is tiff_unpredict_line(unsigned char *line, int width, int comps, int bits)
the function allocates a buffer of size 32 chars (unsigned char left[32]) and subsequently writes unbounded memory of size comps into the buffer.
as comps is not validated, parsing a malicious or otherwise malformed file could result in a stack overflow

To reproduce, use fz_load_tiff on data from the attached document

Crash Dump:
==24994==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7fff84e80580 at pc 0x560dac137bea bp 0x7fff84e80500 sp 0x7fff84e804f0
WRITE of size 1 at 0x7fff84e80580 thread T0
    #0 0x560dac137be9 in tiff_unpredict_line source/fitz/load-tiff.c:195
    #1 0x560dac1418d1 in tiff_decode_samples source/fitz/load-tiff.c:1320
    #2 0x560dac142306 in fz_load_tiff_subimage source/fitz/load-tiff.c:1391
    #3 0x560dac142b97 in fz_load_tiff source/fitz/load-tiff.c:1431
