
Stack overflow in tiff_paste_subsamples_tile

=================================================================
==25501==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7ffed5a0bfb0 at pc 0x5626baf21c33 bp 0x7ffed5a0be50 sp 0x7ffed5a0be40
WRITE of size 4 at 0x7ffed5a0bfb0 thread T0
    #0 0x5626baf21c32 in tiff_paste_subsampled_tile source/fitz/load-tiff.c:509
    #1 0x5626baf234e1 in tiff_decode_strips source/fitz/load-tiff.c:686
    #2 0x5626baf286e8 in tiff_decode_samples source/fitz/load-tiff.c:1310
    #3 0x5626baf29306 in fz_load_tiff_subimage source/fitz/load-tiff.c:1391
    #4 0x5626baf29b97 in fz_load_tiff source/fitz/load-tiff.c:1431

SUMMARY: AddressSanitizer: stack-buffer-overflow source/fitz/load-tiff.c:509 in tiff_paste_subsampled_tile
