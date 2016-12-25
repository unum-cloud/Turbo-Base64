TurboBase64: Turbo Base64 Encoding [![Build Status](https://travis-ci.org/powturbo/TurboBase64.svg?branch=master)](https://travis-ci.org/powturbo/TurboBase64)
===================================

###### **Turbo Base64** Encoding library
- 100% C (C++ compatible headers), without inline assembly
<p>
- No other scalar base64 library encode or decode faster
- Encode or decode more than **3 times** faster as other libraries
- More than 3 GB/s, saturates the fastest SSD drives
- :new: more faster
- Portable library, both 32 and 64 bits supported
- Ready and simple to use library, no hassless dependencies
<p>


------------------------------------------------------------------------

## Benchmark:
CPU: Skylake at 3.7GHz, gcc 6.2
- with [TurboBench](https://github.com/powturbo/TurboBench)
- Single thread
- Realistic and practical benchmark with large binary game assets corpus [pd3d](http://www.cbloom.com/pd3d.7z)

|C Size|ratio%|C MB/s|D MB/s|Name|Description
|--------:|-----:|--------:|--------:|----------------|----------------|------------------------|
|42603868|133.3|**3557**|**3081**|[**TurboB64**](https://github.com/powturbo/TurboBase64)|**TurboBase64**|
|42603868|133.3|1715|1956|[TurboB64s](https://github.com/powturbo/TurboBase64)|**TurboBase64**|
|42603868|133.3|1262|1375|[fb64chromium](https://github.com/lemire/fastbase64)|**Google Chromium base64**|
|42603868|133.3|1675|1167|[fb64scalar](https://github.com/lemire/fastbase64)|**Scalar FastBase64**|
|43269553|135.4| 903|171|[fb64linux](https://github.com/lemire/fastbase64)|**Linux base64**|
|52024524|162.8|1122|816|[fb64quicktime](https://github.com/lemire/fastbase64)|**Apple Quicktime base64**|
|31952900|100.0|**13397.48**|**14447.93**|**memcpy**|

(**bold** = pareto)  MB=1.000.000

<p>

## Compile:

        make

## Usage:

        ./turbob64 file

### Environment:

###### OS/Compiler (32 + 64 bits):
- Linux: GNU GCC (>=4.6)
- clang (>=3.2) 
- Windows: MinGW
- Windows: Visual Studio 2015

###### References:
- [fastbase](https://github.com/lemire/fastbase64)
- [base64simd](https://github.com/WojciechMula/base64simd)
- [base64](https://github.com/aklomp/base64)

Last update: 25 DEC 2016
