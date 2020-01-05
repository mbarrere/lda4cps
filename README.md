# LDA4CPS - Logical Dependency Analyser for Cyber-Physical Systems
### Version 0.62.1


## Contents
- [Requirements](#requirements)
- [Usage](#usage)
- [Execution examples](#execution-examples)
- [Configuration parameters](#configuration-parameters)
- [Licence](#licence)

## Requirements
* Java 8
* Python 2/3
* Optional: Python 3, PuLP, and Gurobi to enable second MaxSAT solver

## Usage

1. ```java -jar lda4cps.jar inputFile.json [-c configFile]```  
This command executes LDA4CPS with an input JSON file that describes the network under analysis.
The method used by LDA4CPS to identify critical nodes is fully described in our paper:
[Identifying Security-Critical Cyber-Physical Components in Industrial Control Systems](https://arxiv.org/abs/1905.04796)

2. ```./web-viewer.py```  
This command launches the webviewer (Python-based HTTP server) that displays the AND/OR graph as well as its critical nodes.
By default, the webviewer reads the file *view/sol.json* and displays it at [http://localhost:8000/viz.html](http://localhost:8000/viz.html)



## Execution example

### Example: Aircraft - Case 1
```
$> java -jar lda4cps.jar examples/aircraft-case1.json
```
```
== LDA4CPS v0.62.1 ==
== Started at 2020-01-05 15:34:52.46 ==

=> Loading problem specification...  done in 255 ms (0 seconds).
----------------------------------
Problem source: _s_
Problem target: PitchMACC
----------------------------------
=> Performing Tseitin transformation...  done in 135 ms (0 seconds).
|+| Solvers: [MaxSAT]

==================================
=> BEST solution found by MAX-SAT-SOLVER for:
Source: _s_
Target: PitchMACC
=== Security Metric ===
Joint probability of failure: 6.3E-6
[+] Failed nodes: none
Total critical nodes: 4
[+] Most likely critical set (MLCS): (SP1,0.05); (SP2,0.05); (SP3,0.05); (SP4,0.05);
[*] Metric computation time: 86 ms (0 seconds).
==================================
Solution saved in: ./view/sol.json
== LDA4CPS ended at 2020-01-05 15:34:52.968 ==
```

##### Run the webviewer:
```
$> ./web-viewer.py
```
```
Running in Python 2...
('Started HTTP server on port ', 8000)
```
In the browser, go to [http://localhost:8000/viz.html](http://localhost:8000/viz.html)  
You should see the following AND/OR graph along with its critical nodes (*a* and *c*):

![Screenshot - simple example](https://github.com/mbarrere/lda4cps/blob/master/screenshots/aircraft-case1.png)

---


## Configuration parameters
The configuration parameters are stored in the file `lda4cps.conf`.
The tool also accepts a different configuration file as argument [-c configFile] to override the configuration in *lda4cps.conf*. If the file `lda4cps.conf` is not present, LDA4CPS uses the default configuration values (see below).

### Solvers
* ```solvers.sat4j = true``` Enables/disables default MaxSAT solver (*default=true*)
* ```solvers.optim = false``` Enables/disables second Gurobi-based MaxSAT solver (*default=false*)

### Python environment
* ```python.path = /usr/local/bin/python3``` Specifies the path to the Python 3 binary (only used with the second [optional] Gurobi-based solver).
* ```python.solver.path = python/optim.py``` Specifies the path to the Gurobi-based solver.

### Output flags
* ```output.sol = true``` Indicates LDA4CPS to output the JSON solution with the critical nodes.
* ```output.wcnf = false``` Enables/disables the specification of the problem in WCNF (DIMACS-like) format (*default=false*). The WCNF file can be used to experiment with other MaxSAT solvers.
* ```output.txt = false``` Enables/disables the specification of the problem in a simple list-based representation file (*default=false*).


### Output folders
* ```folders.output = output``` Specifies the default output folder for *.wcnf* and *.txt* files.
* ```folders.view = view``` Specifies the default view folder where the solution (*sol.json*) is stored.

### Debug
* ```tool.debug = false``` Enables light debugging.
* ```tool.fulldebug = false``` Enables full (heavy) debugging.

## Licence
Apache License 2.0
