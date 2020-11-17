# upx_fuzzing_corpus
Fuzzing corpus for UPX

## crash-0f8631c432561309dc3520bb3c5ddcbe286a6bad

Filed as https://github.com/upx/upx/issues/414.

```
==664968==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60c000065400 at pc 0x0000009feb61 bp 0x7ffdf047fad0 sp 0x7ffdf047fac8
READ of size 1 at 0x60c000065400 thread T0
    #0 0x9feb60 in acc_ua_get_le32(void const*) (/git/upx/fuzz/a.out+0x9feb60)
    #1 0x604393 in PackHeader::fillPackHeader(unsigned char const*, int) (/git/upx/fuzz/a.out+0x604393)
    #2 0x5e9e65 in Packer::readPackHeader(int, bool) (/git/upx/fuzz/a.out+0x5e9e65)
    #3 0x61abbe in PackCom::canUnpack() (/git/upx/fuzz/a.out+0x61abbe)
    #4 0x606b0d in try_unpack(Packer*, void*) (/git/upx/fuzz/a.out+0x606b0d)
    #5 0x60cc83 in PackMaster::visitAllPackers(Packer* (*)(Packer*, void*), InputFile*, options_t const*, void*) (/git/upx/fuzz/a.out+0x60cc83)
    #6 0x611fa0 in PackMaster::getUnpacker(InputFile*) (/git/upx/fuzz/a.out+0x611fa0)
    #7 0x612bc6 in PackMaster::test() (/git/upx/fuzz/a.out+0x612bc6)
    #8 0xa0e5b7 in do_one_file(char const*, char*) (/git/upx/fuzz/a.out+0xa0e5b7)
    #9 0xa0f22b in do_files(int, int, char**) (/git/upx/fuzz/a.out+0xa0f22b)
    #10 0x5d1167 in original_main(int, char**) (/git/upx/fuzz/a.out+0x5d1167)
    #11 0x567b75 in LLVMFuzzerTestOneInput /git/upx/fuzz/unpacker_fuzzer.cc:32:3
    #12 0x46f1c1 in fuzzer::Fuzzer::ExecuteCallback(unsigned char const*, unsigned long) (/git/upx/fuzz/a.out+0x46f1c1)
    #13 0x46ea05 in fuzzer::Fuzzer::RunOne(unsigned char const*, unsigned long, bool, fuzzer::InputInfo*, bool*) (/git/upx/fuzz/a.out+0x46ea05)
    #14 0x470ca7 in fuzzer::Fuzzer::MutateAndTestOne() (/git/upx/fuzz/a.out+0x470ca7)
    #15 0x4719c5 in fuzzer::Fuzzer::Loop(std::Fuzzer::vector<fuzzer::SizedFile, fuzzer::fuzzer_allocator<fuzzer::SizedFile> >&) (/git/upx/fuzz/a.out+0x4719c5)
    #16 0x45f7a8 in fuzzer::FuzzerDriver(int*, char***, int (*)(unsigned char const*, unsigned long)) (/git/upx/fuzz/a.out+0x45f7a8)
    #17 0x488bf2 in main (/git/upx/fuzz/a.out+0x488bf2)
    #18 0x7f701777acc9 in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x26cc9)
    #19 0x433049 in _start (/git/upx/fuzz/a.out+0x433049)
```

## crash-cef58bdb873e0ddc2280ad82393b7fa968168f63

Filed as https://github.com/upx/upx/issues/417.

Without libfuzzer:

```
upx.out -d ./crash-cef58bdb873e0ddc2280ad82393b7fa968168f63
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX git-ae24c5  Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 24th 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
Segmentation fault

```

With libfuzzer:

```
upx.out -d ./crash-cef58bdb873e0ddc2280ad82393b7fa968168f63
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX git-ae24c5  Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 24th 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
p_mach.cpp:1610:36: runtime error: member access within null pointer of type 'const struct Mach_segment_command'
p_mach.cpp:1610:28: runtime error: member access within null pointer of type 'const struct Mach_segment_command'
AddressSanitizer:DEADLYSIGNAL
=================================================================
==3332600==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000028 (pc 0x55bbc92c2146 bp 0x7fff9dc52b90 sp 0x7fff9dc52b60 T0)
==3332600==The signal is caused by a READ memory access.
==3332600==Hint: address points to the zero page.
    #0 0x55bbc92c2146 in acc_ua_get_le64(void const*) /git/upx/src/miniacc.h:6255
    #1 0x55bbc8d4d8b0 in get_le64(void const*) /git/upx/src/bele.h:184
    #2 0x55bbc8d5dc01 in LE64::operator unsigned long long() const /git/upx/src/bele.h:434
    #3 0x55bbc9005279 in PackMachBase<N_Mach::MachClass_64<N_BELE_CTP::LEPolicy> >::canUnpack() /git/upx/src/p_mach.cpp:1610
    #4 0x55bbc91e254f in try_unpack /git/upx/src/packmast.cpp:114
    #5 0x55bbc91f10ba in PackMaster::visitAllPackers(Packer* (*)(Packer*, void*), InputFile*, options_t const*, void*) /git/upx/src/packmast.cpp:225
    #6 0x55bbc91f2362 in PackMaster::getUnpacker(InputFile*) /git/upx/src/packmast.cpp:248
    #7 0x55bbc91f2d24 in PackMaster::unpack(OutputFile*) /git/upx/src/packmast.cpp:266
    #8 0x55bbc92cc8cd in do_one_file(char const*, char*) /git/upx/src/work.cpp:160
    #9 0x55bbc92ce1f3 in do_files(int, int, char**) /git/upx/src/work.cpp:271
    #10 0x55bbc8d5a328 in real_main(int, char**) /git/upx/src/main.cpp:1541
    #11 0x55bbc8d61bae in main /git/upx/src/main_entrypoint.cpp:45
    #12 0x7f5825ad7cc9 in __libc_start_main ../csu/libc-start.c:308
    #13 0x55bbc8ce4849 in _start (/git/upx/src/upx.out+0xa14849)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /git/upx/src/miniacc.h:6255 in acc_ua_get_le64(void const*)
==3332600==ABORTING
```


## testcase-6608718114455552

Filed as https://github.com/upx/upx/issues/425.


```
 ./src/upx.out -d  /tmp/testcase-6608718114455552
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX git-f85b79  Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 24th 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
AddressSanitizer:DEADLYSIGNAL
=================================================================
==2707265==ERROR: AddressSanitizer: SEGV on unknown address 0x61afffff8080 (pc 0x7fef09b41b91 bp 0x7ffea984a5a0 sp 0x7ffea9849d48 T0)
==2707265==The signal is caused by a READ memory access.
    #0 0x7fef09b41b91  (/lib/x86_64-linux-gnu/libc.so.6+0x15fb91)
    #1 0x7fef0a751a8c  (/lib/x86_64-linux-gnu/libasan.so.6+0x3ca8c)
    #2 0x55742cea82cf in upx_strlen /usr/local/google/home/liamjm/git/upx/src/snprintf.cpp:844
    #3 0x55742ce0906e in PeFile::Export::convert(unsigned int, unsigned int) /usr/local/google/home/liamjm/git/upx/src/pefile.cpp:1120
    #4 0x55742ce0ce89 in PeFile::processExports(PeFile::Export*) /usr/local/google/home/liamjm/git/upx/src/pefile.cpp:1219
    #5 0x55742ce28cb0 in PeFile::rebuildExports() /usr/local/google/home/liamjm/git/upx/src/pefile.cpp:2722
    #6 0x55742ce59542 in void PeFile::unpack0<PeFile32::pe_header_t, LE32, unsigned int>(OutputFile*, PeFile32::pe_header_t const&, PeFile32::pe_header_t&, unsigned int, bool) /usr/local/google/home/liamjm/git/upx/src/pefile.cpp:2949
    #7 0x55742ce3398c in PeFile32::unpack(OutputFile*) /usr/local/google/home/liamjm/git/upx/src/pefile.cpp:3120
    #8 0x55742cd9cdc9 in Packer::doUnpack(OutputFile*) /usr/local/google/home/liamjm/git/upx/src/packer.cpp:107
    #9 0x55742cdef60d in PackMaster::unpack(OutputFile*) /usr/local/google/home/liamjm/git/upx/src/packmast.cpp:269
    #10 0x55742cec8d73 in do_one_file(char const*, char*) /usr/local/google/home/liamjm/git/upx/src/work.cpp:160
    #11 0x55742ceca699 in do_files(int, int, char**) /usr/local/google/home/liamjm/git/upx/src/work.cpp:271
    #12 0x55742c956328 in real_main(int, char**) /usr/local/google/home/liamjm/git/upx/src/main.cpp:1541
    #13 0x55742c95dbae in main /usr/local/google/home/liamjm/git/upx/src/main_entrypoint.cpp:45
    #14 0x7fef09a08cc9 in __libc_start_main ../csu/libc-start.c:308
    #15 0x55742c8e0849 in _start (/usr/local/google/home/liamjm/git/upx/src/upx.out+0xa15849)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV (/lib/x86_64-linux-gnu/libc.so.6+0x15fb91)
==2707265==ABORTING
```
