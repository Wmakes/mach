# mach.lang.target.isa.asm.crypto

the aes and carry-less multiply rows run against published answers, reached
through the asm module's testing imports: the FIPS-197 appendix C.1 AES-128
cipher and its inverse, and the GCM specification's test case 2 GHASH
multiplies. each run first asks the processor, so a host without the
extension passes having run nothing

