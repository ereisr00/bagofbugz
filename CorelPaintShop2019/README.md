CVE ID: CVE-2019-6114

An integer overflow in the jp2 parsing library of Corel PaintShop Pro 2019 version 21.0.0.119 allows and attacker to cause a buffer overflow during the parsing of a jp2 file.
This buffer overflow can be used to overwrite adjacent memory (including classes allocated on the heap) and thus lead to code execution.

overflow_write.jp2 is a poc that triggers the integer overflow and causes a large corruption of memory that crashes the program.
