Wild pointer dereference

Commit https://github.com/ArtifexSoftware/mupdf/commit/7d52765c5b8a5c76e459d148cd94dbaf51e562ec removed a "default" else on cinfo.output_component. if cinfo.output_component does not fit to any of the checks, colorspace will remain uninitialized. Under certain circumstances, colorspace can remain uninitialized when returned from extract_icc_profile as well. The pointer to the struct is subsequently passed to additional functions that fail to ensure that bounds are valid. 
