# upx_fuzzing_corpus
Fuzzing corpus for UPX

[TOC]

## crash-0f8631c432561309dc3520bb3c5ddcbe286a6bad
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

