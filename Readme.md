# Designs ran on ASAP7 PDK Platform - Test 1
### Aim: 
To improve the timing parameters below by making changes in CTS script:
1.	Overall Performance Elapsed Runtime
2.	CPU User Time
3.	CTS Elapsed Seconds to run CTS Stage
All the parameters to look are highlighted in bold.
#### Description: 
This repository reflects the default and updated performance runtimes of current designs without any changes made. I plan to improve the overall runtime for a design by modifying the Clock Tree Synthesis TCL script.  
The idea is to modify the OpenROAD scripts for improvement of CTS timings. To do so, I have updated the scripts/cts.tcl file to improve the Overall Performance Elapsed Runtime, CPU User Time, and CTS Elapsed Seconds to run CTS Stage

### Steps taken: 
* Login to VSD Cloud Platform
* Open Terminal >> cd
* Create a new directory >> mkdir `contest` >> git clone `<forked repo>`
* Goto OpenROAD-flow-scripts/flow/scripts
* To edit >> Run >> gedit `cts.tcl`
* Updated the process user settings code `if` loop with `foreach` loop to simplify and optimize the code to see overall runtime improvement.

### Before: During Test 1

    if { [info exists ::env(SETUP_SLACK_MARGIN)] && $::env(SETUP_SLACK_MARGIN) > 0.0} {
      puts "Setup slack margin $::env(SETUP_SLACK_MARGIN)"
      append additional_args " -setup_margin $::env(SETUP_SLACK_MARGIN)"
    }
    if { [info exists ::env(HOLD_SLACK_MARGIN)] && $::env(HOLD_SLACK_MARGIN) > 0.0} {
      puts "Hold slack margin $::env(HOLD_SLACK_MARGIN)"
      append additional_args " -hold_margin $::env(HOLD_SLACK_MARGIN)"
    }

### After: During Test 2

    foreach var {SETUP_SLACK_MARGIN HOLD_SLACK_MARGIN} {
      if {[info exists ::env($var)] && $::env($var) > 0.0} {
        puts "${var} slack margin $::env($var)"
        append additional_args " -${var:l}_margin $::env($var)"
      }
    }
* Save the file.
* And proceed to run all designs as shown below.

## TEST 1: Runtimes of the default GCD design
### GCD design
#### Overall Performance time
`Elapsed time: 0:01.94` [h:]min:sec. `CPU time: user 1.55` sys 0.12 (86%). Peak memory: 322560KB.  
cp results/asap7/gcd/base/6_1_merged.gds results/asap7/gcd/base/6_final.gds  

    Log                       Elapsed seconds  
    3_4_resizer                      204  
    2_3_tdms_place                   126  
    6_1_merge                        154  
    5_2_TritonRoute                 2601  
    3_1_place_gp_skip_io             160  
    2_4_mplace                       136  
    1_1_yosys                        335
    5_1_fastroute                    154
    3_5_opendp                       147
    3_3_place_gp                     298
    2_2_floorplan_io                 156
    6_report                         381
    2_5_tapcell                      136
    2_1_floorplan                    153
    3_2_place_iop                    194
    4_1_cts                          381 <-- To be improved.
    4_2_cts_fillcell                 140
    2_6_pdn                          161

 ![image](https://user-images.githubusercontent.com/42606968/228982365-89686e5b-9042-462f-94fc-1d83fafaff06.png)

#### Timing report from 4_1_cts.log
`Elapsed time: 0:06.21`[h:]min:sec. `CPU time: user 5.47` sys 0.06 (88%). Peak memory: 196980KB.

## TEST 1: Runtimes of the default GCD design
### IBEX Design
#### Overall Performance time

`Elapsed time: 0:03.58`[h:]min:sec. `CPU time: user 3.37` sys 0.15 (98%). Peak memory: 477116KB.

    Log                       Elapsed seconds
    3_4_resizer                     1202
    2_3_tdms_place                   131
    6_1_merge                        238
    5_2_TritonRoute                41126
    3_1_place_gp_skip_io             519
    2_4_mplace                       133
    1_1_yosys                       6099
    5_1_fastroute                   2293
    3_5_opendp                       517
    3_3_place_gp                    4052
    2_2_floorplan_io                 150
    6_report                        4290
    2_5_tapcell                      137
    2_1_floorplan                    262
    3_2_place_iop                    133
    4_1_cts                         6983 <--- to be improved
    4_2_cts_fillcell                 123
    2_6_pdn                          133

![image](https://user-images.githubusercontent.com/42606968/228983391-7e695508-09df-4bea-bdcb-d663800ffdf4.png)

#### Timing report from 4_1_cts.log
`Elapsed time: 1:56.23`[h:]min:sec. `CPU time: user 116.06` sys 0.17 (100%). Peak memory: 296996KB.

## TEST 1: Runtimes of the default GCD design
### RISCV32i Design
#### Overall Performance time
`Elapsed time: 0:03.01`[h:]min:sec. `CPU time: user 2.86` sys 0.15 (100%). Peak memory: 446068KB.
cp results/asap7/riscv32i/base/6_1_merged.gds results/asap7/riscv32i/base/6_final.gds

    Log                       Elapsed seconds
    3_4_resizer                      513
    2_3_tdms_place                   129
    6_1_merge                        181
    5_2_TritonRoute                28521
    3_1_place_gp_skip_io             131
    2_4_mplace                      9082
    1_1_yosys                       1951
    5_1_fastroute                    819
    3_5_opendp                       367
    3_3_place_gp                    2162
    2_2_floorplan_io                 150
    6_report                        2137
    2_5_tapcell                      147
    1_1_yosys_hier_report           2436
    2_1_floorplan                    209
    3_2_place_iop                    146
    4_1_cts                         2045
    4_2_cts_fillcell                 125
    2_6_pdn                          147

![image](https://user-images.githubusercontent.com/42606968/228983294-23eedc26-ef36-4872-ba49-e1b1b24c5459.png)

#### Timing report from 4_1_cts.log
`Elapsed time: 0:34.05`[h:]min:sec. `CPU time: user 33.93` sys 0.11 (99%). Peak memory: 296856KB.
