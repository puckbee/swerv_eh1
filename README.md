# SweRV RISC-V core from Western Digital

This repository contains the SweRV core design RTL

## License

By contributing to this project, you agree that your contribution is governed by [Apache-2.0](LICENSE).  
Files under the [tools](tools/) directory may be available under a different license. Please review individual file for details.

## Directory Structure

    ├── configs                 # Configurations Dir
    │   └── snapshots           # Where generated configuration files are created
    ├── design                  # Design root dir
    │   ├── dbg                 #   Debugger
    │   ├── dec                 #   Decode, Registers and Exceptions
    │   ├── dmi                 #   DMI block
    │   ├── exu                 #   EXU (ALU/MUL/DIV)
    │   ├── ifu                 #   Fetch & Branch Prediction
    │   ├── include             
    │   ├── lib
    │   └── lsu                 #   Load/Store
    ├── docs
    ├── tools                   # Scripts/Makefiles

## Dependencies

- Verilator **(3.926 or later)** must be installed on the system
- If addding/removing instructions, espresso must be installed (used by *tools/coredecode*)

## Quickstart guide
1. Clone the repository
1. Setup RV_ROOT to point to the path in your local filesystem
1. Determine your configuration {optional}
1. Run make with tools/Makefile

### Configurations

SweRV can be configured by running the `$RV_ROOT/configs/swerv.config` script:

`% $RV_ROOT/configs/swerv.config -h` for detailed help options

For example to build with a DCCM of size 64 :  

`% $RV_ROOT/configs/swerv.config -dccm_size=64`  

This will update the **default** snapshot in $RV_ROOT/configs/snapshots/default/ with parameters for a 64K DCCM.  

Add `-snapshot=dccm64`, for example, if you wish to name your build snapshot *dccm64* and refer to it during the build.  

This script derives the following consistent set of include files :  

    $RV_ROOT/configs/snapshots/default
    ├── common_defines.vh                       # `defines for testbench or design
    ├── defines.h                               # #defines for C/assembly headers
    ├── pd_defines.vh                           # `defines for physical design
    ├── perl_configs.pl                         # Perl %configs hash for scripting
    ├── pic_ctrl_verilator_unroll.sv            # Unrolled verilog based on PIC size
    ├── pic_map_auto.h                          # PIC memory map based on configure size
    └── whisper.json                            # JSON file for swerv-iss



### Building a model
1. Set the RV_ROOT environment variable to the root of the SweRV directory structure  

    `RV_ROOT = /path/to/swerv`  
    `export RV_ROOT`

1. Create your configuration  

    *(Skip if default is sufficient)*  
    *(Name your snapshot to distinguish it from the default. Without an explicit name, it will update/override the **default** snapshot)*  
    `$RV_ROOT/configs/swerv.config [configuration options..] -snapshot=mybuild`  

    Snapshots are placed in `$RV_ROOT/configs/snapshots/<snapshot name>/` directory

1. Build with **verilator**:  

    `make -f $RV_ROOT/tools/Makefile verilator [snapshot=name]`  

This will create and populate the verilator *obj_dir/* in the current work dir.  

**Other targets supported**:  

    vcs  (Synopsys)  
    irun (Cadence)  
