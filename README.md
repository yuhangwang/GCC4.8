# GCC4.8
**GNU Compiler Collection (GCC) 4.8**

This is an unofficial verbatim redistribution of the binary&source form of GCC 4.8 under its open source license terms (see the LICENSE file).

This redistribution is under the same license as the original.

Please visit the official website for more details: https://gcc.gnu.org


## Download
Due to the file size restriction of GitHub, the repository has been moved to Bitbucket:
You can download the compiled binary files at the https://bitbucket.org/yuhangwang/gcc-4.8

## Installation Notes
### Order of installation
* GMP 4.3.2
* PPL 0.11 (requires GMP 4.3.2 with C++ support)
* MPFR 2.4.2 (requires GMP 4.3.2)
* MPC 0.8.1 (requires GMP 4.3.2 & MPFR 2.4.2)
* CLooG 0.15.11 (requires GMP 4.3.2 & PPL 0.11)
* GCC 4.8.4 (requires all above)

### Compilation

#### [1] Add external library into system paths
Since all the prerequisite external libraries are installed in non-system locations,
the system variable **LD_LIBRARY_PATH** must be modified so that the linker can find
those binary library files.

##### BASH shell users
For BASH shell, add the following to a file called "tmp.bashrc":
```bash
base_dir=/Scr/scr-test-steven/install
gmp_dir=${base_dir}/libgmp/4.3.2
mpfr_dir=${base_dir}/libmpfr/2.4.2
mpc_dir=${base_dir}/libmpc/0.8.1
cloog_dir=${base_dir}/libcloog/0.18.0
ppl_dir=${base_dir}/libisl/0.11.1

export LD_LIBRARY_PATH="${gmp_dir}/lib:${LD_LIBRARY_PATH}"
export LD_LIBRARY_PATH="${mpfr_dir}/lib:${LD_LIBRARY_PATH}"
export LD_LIBRARY_PATH="${mpc_dir}/lib:${LD_LIBRARY_PATH}"
export LD_LIBRARY_PATH="${cloog_dir}/lib:${LD_LIBRARY_PATH}"
export LD_LIBRARY_PATH="${ppl_dir}/lib:${LD_LIBRARY_PATH}"
```

Then type the following command in the terminal window:
```bash
source tmp.bashrc
```

##### CSH shell users
For CSH shell, add the following to a file named "tmp.cshrc":
```csh
set base_dir=/Scr/scr-test-steven/install
set gmp_dir=${base_dir}/libgmp/4.3.2
set mpfr_dir=${base_dir}/libmpfr/2.4.2
set mpc_dir=${base_dir}/libmpc/0.8.1
set cloog_dir=${base_dir}/libcloog/0.18.0
set ppl_dir=${base_dir}/libisl/0.11.1

setenv LD_LIBRARY_PATH "${gmp_dir}/lib:${LD_LIBRARY_PATH}"
setenv LD_LIBRARY_PATH "${mpfr_dir}/lib:${LD_LIBRARY_PATH}"
setenv LD_LIBRARY_PATH "${mpc_dir}/lib:${LD_LIBRARY_PATH}"
setenv LD_LIBRARY_PATH "${cloog_dir}/lib:${LD_LIBRARY_PATH}"
setenv LD_LIBRARY_PATH "${ppl_dir}/lib:${LD_LIBRARY_PATH}"
```
Then type the following command in the terminal window:
```bash
source tmp.cshrc
```

##### Or use [Modules](http://modules.sourceforge.net)
A better way is to install [Modules](http://modules.sourceforge.net) and create a module files like this:
```tcl
#%Module 1.0

set baseDir "/Scr/scr-test-steven/install/libmpc/0.8.1"
prepend-path "PATH" "${baseDir}/bin"
prepend-path "LD_LIBRARY_PATH" "${baseDir}/lib"
prepend-path "INCLUDE" "${baseDir}/include"
```
Make a new folder /home/steven/local/modules and save this into the new folder as a new file "libmpc-0.8.1".
Then type the following in the terminal:
```bash
module use --append /home/steven/local/modules
```
The above will add **/home/steven/local/modules** to the searching path for the **module** command.

To change the environment variable, type:
```bash
module add libmpc-0.8.1
```

To reverse the change, type:
```bash
module rm libmpc-0.8.1
```

#### [2] Compile, Check & Install
The following bash script is used:
```bash
#!/bin/sh
GCC_VERSION=4.8.4

base_dir=/Scr/scr-test-steven/install
target_dir=${base_dir}/gcc/${GCC_VERSION}
mkdir -p $target_dir

gcc_base_dir=${base_dir}/gcc/4.7.4
cc=${gcc_base_dir}/bin/gcc
cxx=${gcc_base_dir}/bin/g++

gmp_dir=${base_dir}/libgmp/4.3.2
mpfr_dir=${base_dir}/libmpfr/2.4.2
mpc_dir=${base_dir}/libmpc/0.8.1
cloog_dir=${base_dir}/libcloog/0.18.0
ppl_dir=${base_dir}/libisl/0.11.1



../gcc-${GCC_VERSION}/configure CC=$cc CXX=$cxx CPPFLAGS=-I${gcc_base_dir}/include LDFLAGS="-L${gcc_base_dir}/lib64" --prefix=$target_dir --disable-multilib --with-gmp=$gmp_dir --with-mpfr=$mpfr_dir --with-mpc=$mpc_dir --with-cloog=$cloog_dir --with-isl=$isl_dir

```

Then the following steps were taken:
```bash

make -j10
make -k check -j10 | tee QualityVerification.txt
../gcc-4.8.4/contrib/test_summary -o | tee Summary_check_gcc-4.8.4.txt
make install
```

### Portability
The following dynamic library files have been added to the **lib64/** subfolder to increase the portability of the binary form of GCC:
* libcloog-isl.so.4 (copied from /home/steven/install/libcloog/0.18.0/libcloog-isl.so.4.0.0)
* libisl.so.10  (copied from /home/steven/install/libisl/libisl.so.10.1.1)

### Quality verification
See the "QualityVerification.txt" and "Summary-check-gcc-4.8.4.txt" for the output of "make check".
Here are the most important results:

- === gcc Summary ===
	- of expected passes		95006
	- of expected failures		267
	- of unsupported tests		1607

- === gfortran Summary ===
	- of expected passes		44002
	- of unexpected failures	6
	- of expected failures		56
	- of unsupported tests		73

- === g++ Summary ===
	- of expected passes		54566
	- of expected failures		292
	- of unsupported tests		921

- === libstdc++ Summary ===
	- of expected passes		9291
	- of expected failures		45
	- of unsupported tests		217