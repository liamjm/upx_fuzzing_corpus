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
    #2 0x55742cea82cf in upx_strlen /git/upx/src/snprintf.cpp:844
    #3 0x55742ce0906e in PeFile::Export::convert(unsigned int, unsigned int) /git/upx/src/pefile.cpp:1120
    #4 0x55742ce0ce89 in PeFile::processExports(PeFile::Export*) /git/upx/src/pefile.cpp:1219
    #5 0x55742ce28cb0 in PeFile::rebuildExports() /git/upx/src/pefile.cpp:2722
    #6 0x55742ce59542 in void PeFile::unpack0<PeFile32::pe_header_t, LE32, unsigned int>(OutputFile*, PeFile32::pe_header_t const&, PeFile32::pe_header_t&, unsigned int, bool) /git/upx/src/pefile.cpp:2949
    #7 0x55742ce3398c in PeFile32::unpack(OutputFile*) /git/upx/src/pefile.cpp:3120
    #8 0x55742cd9cdc9 in Packer::doUnpack(OutputFile*) /git/upx/src/packer.cpp:107
    #9 0x55742cdef60d in PackMaster::unpack(OutputFile*) /git/upx/src/packmast.cpp:269
    #10 0x55742cec8d73 in do_one_file(char const*, char*) /git/upx/src/work.cpp:160
    #11 0x55742ceca699 in do_files(int, int, char**) /git/upx/src/work.cpp:271
    #12 0x55742c956328 in real_main(int, char**) /git/upx/src/main.cpp:1541
    #13 0x55742c95dbae in main /git/upx/src/main_entrypoint.cpp:45
    #14 0x7fef09a08cc9 in __libc_start_main ../csu/libc-start.c:308
    #15 0x55742c8e0849 in _start (/git/upx/src/upx.out+0xa15849)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV (/lib/x86_64-linux-gnu/libc.so.6+0x15fb91)
==2707265==ABORTING
```

## testcase-5369669307465728

Filed as https://github.com/upx/upx/issues/426.

```
upx.out -d /tmp/testcase-5369669307465728 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX git-f85b79  Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 24th 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
=================================================================
==2711255==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61d000000830 at pc 0x7fdf8278a983 bp 0x7fff35283570 sp 0x7fff35282d20
READ of size 72 at 0x61d000000830 thread T0
    #0 0x7fdf8278a982 in __interceptor_memcpy (/lib/x86_64-linux-gnu/libasan.so.6+0x39982)
    #1 0x55e6709a6b0c in PackMachBase<N_Mach::MachClass_64<N_BELE_CTP::LEPolicy> >::unpack(OutputFile*) /git/upx/src/p_mach.cpp:1433
    #2 0x55e670b55dc9 in Packer::doUnpack(OutputFile*) /git/upx/src/packer.cpp:107
    #3 0x55e670ba860d in PackMaster::unpack(OutputFile*) /git/upx/src/packmast.cpp:269
    #4 0x55e670c81d73 in do_one_file(char const*, char*) /git/upx/src/work.cpp:160
    #5 0x55e670c83699 in do_files(int, int, char**) /git/upx/src/work.cpp:271
    #6 0x55e67070f328 in real_main(int, char**) /git/upx/src/main.cpp:1541
    #7 0x55e670716bae in main /git/upx/src/main_entrypoint.cpp:45
    #8 0x7fdf81a44cc9 in __libc_start_main ../csu/libc-start.c:308
    #9 0x55e670699849 in _start (/git/upx/src/upx.out+0xa15849)

0x61d000000830 is located 0 bytes to the right of 1968-byte region [0x61d000000080,0x61d000000830)
allocated by thread T0 here:
    #0 0x7fdf827fc7a7 in operator new[](unsigned long) (/lib/x86_64-linux-gnu/libasan.so.6+0xab7a7)
    #1 0x55e6709a5f0b in PackMachBase<N_Mach::MachClass_64<N_BELE_CTP::LEPolicy> >::unpack(OutputFile*) /git/upx/src/p_mach.cpp:1421
    #2 0x55e670b55dc9 in Packer::doUnpack(OutputFile*) /git/upx/src/packer.cpp:107
    #3 0x55e670ba860d in PackMaster::unpack(OutputFile*) /git/upx/src/packmast.cpp:269
    #4 0x55e670c81d73 in do_one_file(char const*, char*) /git/upx/src/work.cpp:160
    #5 0x55e670c83699 in do_files(int, int, char**) /git/upx/src/work.cpp:271
    #6 0x55e67070f328 in real_main(int, char**) /git/upx/src/main.cpp:1541
    #7 0x55e670716bae in main /git/upx/src/main_entrypoint.cpp:45
    #8 0x7fdf81a44cc9 in __libc_start_main ../csu/libc-start.c:308

SUMMARY: AddressSanitizer: heap-buffer-overflow (/lib/x86_64-linux-gnu/libasan.so.6+0x39982) in __interceptor_memcpy
Shadow bytes around the buggy address:
  0x0c3a7fff80b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff80c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff80d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff80e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c3a7fff80f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c3a7fff8100: 00 00 00 00 00 00[fa]fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8110: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8120: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8130: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8140: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c3a7fff8150: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
  Shadow gap:              cc
==2711255==ABORTING

```

## testcase-5924189438607360

``upx.out -v -d /tmp/testcase-5924189438607360
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX git-ae607f+ Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 24th 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
upx.out: /tmp/testcase-5924189438607360: CantUnpackException: corrupted resources

Unpacked 1 file: 0 ok, 1 error.

=================================================================
==1266283==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 1 byte(s) in 1 object(s) allocated from:
    #0 0x7f7bceb127a7 in operator new[](unsigned long) (/lib/x86_64-linux-gnu/libasan.so.6+0xab7a7)
    #1 0x55bcf568baf5 in PeFile::Resource::build() /git/upx/src/pefile.cpp:1734
    #2 0x55bcf56a3829 in PeFile::rebuildResources(unsigned char*&, unsigned int) /git/upx/src/pefile.cpp:2760
    #3 0x55bcf56d2a9a in void PeFile::unpack0<PeFile32::pe_header_t, LE32, unsigned int>(OutputFile*, PeFile32::pe_header_t const&, PeFile32::pe_header_t&, unsigned int, bool) /git/upx/src/pefile.cpp:2960
    #4 0x55bcf56ab998 in PeFile32::unpack(OutputFile*) /git/upx/src/pefile.cpp:3120
    #5 0x55bcf5614dd5 in Packer::doUnpack(OutputFile*) /git/upx/src/packer.cpp:107
    #6 0x55bcf5667619 in PackMaster::unpack(OutputFile*) /git/upx/src/packmast.cpp:269
    #7 0x55bcf5740d7f in do_one_file(char const*, char*) /git/upx/src/work.cpp:160
    #8 0x55bcf57426a5 in do_files(int, int, char**) /git/upx/src/work.cpp:271
    #9 0x55bcf51c9446 in main /git/upx/src/main.cpp:1538
    #10 0x7f7bcdd5acc9 in __libc_start_main ../csu/libc-start.c:308

SUMMARY: AddressSanitizer: 1 byte(s) leaked in 1 allocation(s).
`
```
