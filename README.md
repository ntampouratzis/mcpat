# cMcPAT - Andreas modified version. 

This repository contains a version of McPAT that is based on the modified version of McPAT employed in the [H2020 COSSIM](https://github.com/H2020-COSSIM) simulation framework. 

This cMcPAT version can be used indepently from the COSSIM framework. Actually it is strongly advisable not to use it in the context of COSSIM as some of the changes made here make it compatible with newer versions of gem5, than the one used in COSSIM and therefore issues might arise.


## Differences between cMcPAT and official McPAT v1.3

### simpleCPU models of gem5

McPAT is developed with certain processor types under consideration. As such it makes a number of assumptions about the presence, size and functioning of different processor components that may not be used in systems using simpleCPU models in gem5. For example, for simpleCPU models, gem5 does not model or report several (micro)architectural structures that are typically found in a processor and therefore the output configuration files (config) do not provide enough information for McPAT. Additionally, by not employing certain structures or using structures with very small sizes, McPAT either reports errors or crashes in such cases.

cMcPAT is modified to handle those cases. On top of that, it adds two Processor Description templates that can be used when trying to model ARM and x86 simpleCPU models from gem5. 

### Improving the accuracy of McPAT

A number of changes have been made to technology specific parameters. The following table summarizes the differences made to the technology.cc file of the cacti component for the Empirical undifferentiated core / FU coefficient :

Technology Node | McPAT v1.3 | cMcPAT
------------ | ------------- | -------------
180nm | 1.5 | 1.0/1.5
90nm | 1 | 1.0/(0.7 * 0.7)
65nm | 0.7 | 1.0/0.7
45nm | 0.7 * 0.7 | 1
32nm | 0.7 * 0.7 * 0.7 | 0.7
22nm | 0.7 * 0.7 * 0.7 * 0.7 | 0.7 * 0.7
16nm | 0.7 * 0.7 * 0.7 * 0.7 * 0.7 | 0.7 * 0.7 *0.7

These changes have been proposed in the paper: 

Xi, Sam Likun, Hans Jacobson, Pradip Bose, Gu-Yeon Wei, and David Brooks. _"Quantifying sources of error in McPAT and potential impacts on architectural studies."_ In 2015 IEEE 21st International Symposium on High Performance Computer Architecture (HPCA), pp. 577-589. IEEE, 2015. 

To validate their effect, cMcPAT has been run against the number reported in the official paper of McPAT (S. Li, J.-H. Ahn,_"McPAT: An integrated power, area, and timing modeling framework for multicore and manycore architectures"_, in Microarchitecture, 2009. MICRO-42. 42nd Annual IEEE/ACM International Symposium on, (pp. 469–480)) using the templates provided in the v1.3 release of McPAT. The table below summarizes the effect of the changes

Processor | Published total power and area | McPAT v1.3 Results | cMcPAT Results | Estimation Improvement (negative numbers indicate a worse result by the cMcPAT)
------------ | ------------- | ------------- | ------------- | -------------
Niagara | 63W / 378mm<sup>2</sup> | 52.59W / 269.70mm<sup>2</sup> | 56.91W / 302.53mm<sup>2</sup> | **41,5%** / **30,3%**
Niagara 2 | 84W / 342mm<sup>2</sup> | 72.97W / 262.51mm<sup>2</sup> | 76.54W / 279.01mm<sup>2</sup> | **32,3%** / **20,8%**
Alpha 21364 | 125W / 396mm<sup>2</sup> | 86.03W / 311.69mm<sup>2</sup> | 85.86W / 299.4mm<sup>2</sup> | -0,4% / -14,6%
Xeon Tulsa | 150W / 435mm<sup>2</sup> | 134.94W / 411mm<sup>2</sup> | 146.33W / 432mm<sup>2</sup> |**75,6%** / **87,5%**
Cortex A9 | 1.9W / 6.7mm<sup>2</sup> | 1.74W / 5.40mm<sup>2</sup> | 1.79W / 5.96mm<sup>2</sup> | **31,3%** / **43,1%**

## Differences between my version of cMcPAT and official cMcPAT

This version assumes that the gem5 version used is the latest (Nov. 2019) available in the [official gem5 repository](https://gem5.googlesource.com/public/gem5). Compared to the main cMcPAT's source code, minor changes have been made, however the script that is used to construct proper mcpat input xml files using the results of the gem5 simulation has been updated. In addition to the simpleCPU models support as specified in the ARM_AtomicSimpleCPU_template.xml and x86_AtomicSimpleCPU_template.xml, a new template - inorder_arm.xml has been added. This template can be used with the minorCPU model for ARM ISA of gem5.


## Compiling and executing cMcPAT

Please read the [README](mcpat/README) file included in the mcpat folder.

If you experience any issues/errors when trying to compile mcpat, then installing g++-multilib and libc6-dev-i386 should resolve those issues. 
Assuming Ubuntu 19.10 where I was able to test it, type:
sudo apt install libc6-dev-i386
sudo apt install gcc-7-multilib g++-7-multilib  (replace 7 with the version of gcc/g++ you are using)


## Using gem5 statistics and configuration files to make proper cMcPAT inputs

Please check the GEM5toMcPAT script included in the Scripts folder.

## Energy Reporting

Please check the print_energy script included in the Scripts folder.

## Licensing

Refer to the [LICENSE](LICENSE) and [COPYING](COPYING.md) files included. Individual license may be present in different files in the source codes.

#### Authors

* Andreas Brokalakis (kingmouf@gmail.com)

Please contact for any questions.



