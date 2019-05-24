function tiff_read_tag reads user input and initializes jpegtables and jpegtableslen without properly validating the input.
These variables are used in subsequent function calls without sufficient checks.
The result is that arbitrary memory can be read/written or that the software can  behave in unexpected ways

AddressSanitizer:DEADLYSIGNAL
=================================================================
==11635==ERROR: AddressSanitizer: SEGV on unknown address 0x7f72548ef900 (pc 0x55df0a484e8b bp 0x7ffd195cf230 sp 0x7ffd195cf200 T0)
==11635==The signal is caused by a READ memory access.
    #0 0x55df0a484e8a in first_marker thirdparty/libjpeg/jdmarker.c:1063
    #1 0x55df0a484f9b in read_markers thirdparty/libjpeg/jdmarker.c:1096
    #2 0x55df0a480039 in consume_markers thirdparty/libjpeg/jdinput.c:568
    #3 0x55df0a474f75 in jpeg_consume_input thirdparty/libjpeg/jdapimin.c:302
    #4 0x55df0a474e7e in jpeg_read_header thirdparty/libjpeg/jdapimin.c:250
    #5 0x55df0a327518 in next_dctd source/fitz/filter-dct.c:202
    #6 0x55df0a2c4f95 in fz_available include/mupdf/fitz/stream.h:173
    #7 0x55df0a2c5282 in fz_read source/fitz/stream-read.c:26
    #8 0x55df0a2a81db in tiff_decode_data source/fitz/load-tiff.c:391
    #9 0x55df0a2a9251 in tiff_decode_strips source/fitz/load-tiff.c:709
    #10 0x55df0a2ab1c9 in tiff_decode_samples source/fitz/load-tiff.c:1310
    #11 0x55df0a2ab558 in fz_load_tiff_subimage source/fitz/load-tiff.c:1391
    #12 0x55df0a2ab789 in fz_load_tiff source/fitz/load-tiff.c:1431

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV thirdparty/libjpeg/jdmarker.c:1063 in first_marker
==11635==ABORTING
