# Decompilers in practice

We list some decompiler projects below, and these tools can be downloaded from their offical website. The readers can also try out each tool online
at https://dogbolt.org. If there are any questions, please submit PR. We will follow their change log and update this page.

|Decompiler|ProjectStatus|Platform|
|---|---|---|
|Hex-Rays|TODO|TODO|
|Ghidra|TODO|TODO|


### Hex-Rays
Hex-Rays is a plugin of famous binary anlysis platform - IDA pro. 
The Hex-Rays Decompiler brings binary software analysis within reach of millions of programmers. 
It converts native processor code into a readable C-like pseudocode text.

Currently the decompiler supports compiler generated code for the x86, x64, ARM32, ARM64, and PowerPC processors. 
We plan to port it to other platforms in the future. 
The programmatic API allows our customers to improve the decompiler output. 
Vulnerability search, software validation, coverage analysis are the directions that immediately come to mind.

The decompiler runs on MS Windows, Linux, and Mac OS X. The GUI and text IDA versions are supported.

**Last update:** IDA 8.1.221006 October 6, 2022

**Url:** https://hex-rays.com/decompiler/

### Ghidra
Ghidra is a software reverse engineering (SRE) framework created and maintained by the National Security Agency Research Directorate. 
This framework includes a suite of full-featured, 
high-end software analysis tools that enable users to analyze compiled code on a variety of platforms including Windows, macOS, and Linux. 
Capabilities include disassembly, assembly, **decompilation**, graphing, and scripting, along with hundreds of other features. 
Ghidra supports a wide variety of processor instruction sets and executable formats and can be run in both user-interactive and automated modes. 
Users may also develop their own Ghidra extension components and/or scripts using Java or Python.

**Last update:** Ghidra 10.2 Nov 3, 2022

**Url:** https://github.com/NationalSecurityAgency/ghidra.git

### JEB
JEB is a reverse-engineering platform to perform disassembly, decompilation, debugging, and analysis of code and document files, 
manually or as part of an analysis pipeline. 

It supports many platform including Dalvik, Java, Intel x86, Intel x86-64, ARM, ARM64, MIPS, MIPS64, RISC-V, S7 PLC Block, WebAssembly, 
EVM (Ethereum Decompiler for Smart Contracts), Diem decompiler.

**Last update:** November, 2022

**Url:** https://www.pnfsoftware.com/jeb/

### RetDec
RetDec is a retargetable machine-code decompiler based on LLVM. 
The decompiler is not limited to any particular target architecture, operating system, or executable file format: 
Supported file formats: ELF, PE, Mach-O, COFF, AR (archive), Intel HEX, and raw machine code. 
Supported architectures (32b only): Intel x86, ARM, MIPS, PIC32, and PowerPC.

It aims to output recompilable-c-code.

**Last update:** Apr 8, 2020

**Url:** https://github.com/avast/retdec.git

### rev.ng
It is a tool for binary analysis, reverse engineering, and binary translation. 
We are reimagining what you can expect from reverse engineering tools, in terms of capabilities, usability, and toolability.

They are developing a full-fledged decompiler based on rev.ng, featuring an interactive UI. 
Check out the features and stay tuned for the coming beta.

**Last update:** Nov 11, 2022

**Url:** https://rev.ng/

### Binary Ninja 
Binary Ninja is an interactive disassembler, decompiler, and binary analysis platform for 
reverse engineers, malware analysts, vulnerability researchers, and software developers that runs on Windows, macOS, and Linux.

Its built-in decompiler works with all of their officially supported architectures at one price and builds on a powerful family of ILs called BNIL. 
In fact, not just their architectures, but even community architectures can produce amazing decompilation. 
Its decompiler outputs to both C and BNIL and can be switched on-demand.

**Last update:** 3.2.3814 Oct 28, 2022

**url:** https://binary.ninja/

### Boomerang
This project is an attempt to develop a real decompiler for machine code programs through the open source community. 
A decompiler takes as input an executable file, and attempts to create a high level, compilable, possibly even maintainable source file that 
does the same thing. It is therefore the opposite of a compiler, which takes a source file and makes an executable. 
However, a general decompiler does not attempt to reverse every action of the decompiler, 
rather it transforms the input program repeatedly until the result is high level source code. 
It therefore won't recreate the original source file; probably nothing like it. 
It does not matter if the executable file has symbols or not, or was compiled from any particular language. 
(However, declarative languages like ML are not considered.)

**Last update:** Oct 28, 2012

**url:** https://boomerang.sourceforge.net/

### dewolf
dewolf is a research decompiler developed during a research cooperation 
from 2019 to 2021 between Germany (Fraunhofer FKIE) and Singapore (DSO National Laboratories).

The restructuring of dewolf is based on the former DREAM/DREAM++ approach [Yakdan et al. NDSS 2015, IEEE (SP) 2016].

The decompiler dewolf is implemented as a plugin for Binary Ninja and uses their Medium-Level intermediate language as the starting point. 
Although they consider dewolf to be pretty stable, it is still a research prototype and not extensively optimized for production use. 
Consequently, you will likely observe a few bugs or even decompilation failures when applying dewolf on real-world binaries.

**Last update:** Nov 10, 2022

**url:** https://github.com/fkie-cad/dewolf

### REC Studio
It is a reverse engineering compiler. 

**last update:** September 24, 2015

**url:** http://backerstreet.com/rec/recdload.htm

### Reko
Reko (Swedish: "decent, obliging") is a decompiler for machine code binaries. This project is freely available under the GNU General Public License.

The project consists of front ends, core decompiler engine, and back ends to help it achieve its goals. 
A command-line, a Windows GUI, and a ASP.NET front end exist at the time of writing. 
The decompiler engine receives inputs from the front ends in the form of either individual executable files or decompiler project files. 
Reko project files contain additional information about a binary file, helpful to the decompilation process or for formatting the output. 
The decompiler engine then proceeds to analyze the input binary.

**Last update:** Apr 8, 2022

**url:** https://github.com/uxmal/reko.git

### Relyze
Relyze Desktop lets you reverse engineer, decompile and diff x86, x64, ARM32 and ARM64 software.
Relyze lets you quickly understand a programs behavior by emitting a high level pseudo code for a function. 
The decompiler is fully interactive, letting you rename and retype variables, navigate variable references and more.

**Last update:** Aug 10, 2022

**url:** https://www.relyze.com/overview.html

### Snowman
Snowman is a native code to C/C++ decompiler, supporting x86, AMD64, and ARM architectures. 
You can use it as a standalone GUI application, a command-line tool, an IDA plug-in, a radare2 plug-in, an x64dbg plug-in, or a library. 
Snowman is free software.

**Last update:** Jun 22, 2019

**Url:** https://www.relyze.com/overview.html
