---
title: "Modulefile Format"
weight: 20
---
We showed previously modulefiles must begin with the magic cookie `#%Module`, but what do modulefiles consist of?
Modulefiles are [Tcl](https://github.com/tcltk/tcl) scripts that modify a user's environment.
Let's look at my `rust` module as an example.
```bash
# contents of /projects/mohimanilab/mustafa/tools/modulefiles/rust
#%Module
##
## Rust modulefile
##
## modulefiles/
##

set ver 1.39.0
set msg "This module adds necessary paths and variables to use rust"

proc ModulesHelp { } {
    puts stderr $msg
}

module-whatis "Use rust $ver"

prepend-path PATH /projects/mohimanilab/mustafa/cargo/bin
```
This is a very simple modulefile. It sets the variable `ver` to `1.39.0`, adds a `whatis` definition and then simply prepends the path to my rust installation to `PATH`.
You can get a summary of what a module does with the `module show` command.
```bash
module show rust
# output:
#
# ------------------------------------------------------------------
# /projects/mohimanilab/mustafa/tools/modulefiles/rust:
# 
# module-whatis    Use rust 1.39.0
# prepend-path     PATH /projects/mohimanilab/mustafa/cargo/bin
# -------------------------------------------------------------------
module show db-search-tools
# output:
#
# -------------------------------------------------------------------
# /projects/mohimanilab/mustafa/tools/modulefiles/db-search-tools:
# 
# module           load rust
# module-whatis    Use MAGMa+, CFM-ID, and CSI:FingerID
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/time-1.9/bin
# setenv           MAGMAPLUS_CLASSIFIER_PATH /projects/mohimanilab/mustafa/tools/magma+/MAGMa-plus
# setenv           MAGMA_ROOT /projects/mohimanilab/mustafa/tools/magma+/MAGMa-plus
# setenv           JAVA_HOME /projects/mohimanilab/mustafa/tools/java/jdk-11.0.7+10-jre/
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/magma+/bin
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/boost/boost_1_59_0
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/rdkit/RDKit_2013_09_1/lib
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/lp_solve/lp_solve_5.5/lpsolve55/bin/ux64
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/liblbfgs/lib
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/cfm-id/cfm/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/java/jdk-11.0.7+10-jre/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/csi-fingerid/sirius-linux64-headless-4.4.29/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/dereplicator/cycloquest_minimal
# setenv           DEREP_BASE /projects/mohimanilab/mustafa/dereplicator/cycloquest_minimal
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/search_runner/target/release
# -------------------------------------------------------------------
```
The most commonly used commands are `setenv` which sets an environment variable that is active when the module is loaded and `prepend-path` which adds a directory to a `PATH`-like variable when the module is loaded.
