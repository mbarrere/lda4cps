# LDA4CPS - Logical Dependency Analyser for Cyber-Physical Systems
### Version 0.62.1


## Contents
- [Summary](#summary)
- [Requirements](#requirements)
- [Usage](#usage)
- [Execution examples](#execution-examples)
- [Configuration parameters](#configuration-parameters)
- [Licence](#licence)

## Summary
Cyber-Physical Systems (CPS) often involve complex networks of interconnected software and hardware components that are logically combined to achieve a common goal or mission, for example, keeping a plane on the air or providing energy to a city. Failures on these components may jeopardise the mission of the system. Therefore, identifying the minimal set of critical CPS components that is most likely to fail and prevent the global system from delivering its mission becomes essential to ensure reliability. 

LDA4CPS is a Java-based tool that has been designed to identify the most likely mission-critical component set (MLCS) using AND/OR dependency graphs enriched with independent failure probabilities. We address the problem from a logical satisfiability perspective, more specifically, as a Weighted Partial MaxSAT problem. Probabilities are translated into a negative logarithmic space in order to linearise the problem within MaxSAT. 

The identification of MLCS in cyber-physical systems provides support to reason about the strength of a system’s design. 
Therefore, LDA4CPS can be used to help automate the evaluation of potential designs over a space of different system configurations.
LDA4CPS studies the robustness of a mission design from a dependency analysis point of view.
The tool includes examples of aircraft dependency models using AND/OR graphs and failure probabilities. 
While the examples are mostly focused on complex aircraft systems, LDA4CPS is abstract enough to deal with AND/OR graph-based models representing other kinds of mission-critical cyber-physical systems.


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
