# LDA4CPS - Logical Dependency Analyser for Cyber-Physical Systems
### Version 0.62.5


## Contents
- [Summary](#summary)
- [Requirements](#requirements)
- [Usage](#usage)
- [Execution example](#execution-example)
- [Configuration parameters](#configuration-parameters)
- [Licence](#licence)

## Summary
Cyber-Physical Systems (CPS) often involve complex networks of interconnected software and hardware components that are logically combined to achieve a common goal or mission, for example, keeping a plane on the air or providing energy to a city. Failures on these components may jeopardise the mission of the system. Therefore, identifying the minimal set of critical CPS components that is most likely to fail and prevent the global system from delivering its mission becomes essential to ensure reliability.

LDA4CPS is a Java-based tool, built on top of [META4ICS](https://github.com/mbarrere/meta4ics), that has been designed to identify the *most likely mission-critical component set (MLCS)* using *AND/OR dependency graphs* enriched with *independent failure probabilities*. We address the problem from a logical satisfiability perspective, more specifically, as a Weighted Partial MaxSAT problem. Probabilities are translated into a negative logarithmic space in order to linearise the problem within MaxSAT.

The identification of MLCS in cyber-physical systems provides support to reason about the strength of a system’s design.
Therefore, LDA4CPS can be used to help automate the evaluation of potential designs over a space of different system configurations.
We study the robustness of a mission design from a dependency analysis point of view.
The tool includes examples of aircraft dependency models using AND/OR graphs and failure probabilities.
While the examples are mostly focused on complex aircraft systems, LDA4CPS is abstract enough to deal with AND/OR graph-based models representing other kinds of mission-critical cyber-physical systems.


## Requirements
* Java 8
* Python 2/3
* Optional: Python 3, PuLP, and Gurobi to enable second MaxSAT solver

## Usage

1. ```java -jar lda4cps.jar inputFile.json [-c configFile]```  
This command executes LDA4CPS with an input JSON file that describes the dependency network under analysis.

2. ```./web-viewer.py```  
This command launches the webviewer (Python-based HTTP server) that displays the AND/OR graph as well as the most likely critical set.
By default, the webviewer reads the file *view/sol.json* and displays it at [http://localhost:8000/viz.html](http://localhost:8000/viz.html)



## Execution example

### Aircraft system - Case 1
```
$> java -jar lda4cps.jar examples/aircraft-case1.json
```
```
== LDA4CPS v0.62.5 ==
== Started at 2020-05-25 01:51:49.326 ==

=> Loading problem specification...  done in 277 ms (0 seconds).
----------------------------------
Problem source: _s_
Problem target: PitchMACC
----------------------------------
=> Performing Tseitin transformation...  done in 141 ms (0 seconds).
|+| Solvers: [MaxSAT]

==================================
=> BEST solution found by MAX-SAT-SOLVER for:
Source: _s_
Target: PitchMACC
=== Security Metric ===
Joint probability of failure: 0.0016
[+] Failed nodes: none
Total critical nodes: 2
[+] Most likely mission-critical set (MLMCS): (LH,0.04); (RH,0.04);
[*] Metric computation time: 91 ms (0 seconds).
==================================
Solution saved in: ./view/sol.json
== LDA4CPS ended at 2020-05-25 01:51:49.869 ==
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
You should see the following AND/OR graph along with the MLMCS (*LH*, *RH*):

![Screenshot - aircraft case 1](https://github.com/mbarrere/lda4cps/blob/master/screenshots/aircraft-case1.png)



### Aircraft system - Case 2
```
$> java -jar lda4cps.jar examples/aircraft-case2.json
```
```
== LDA4CPS v0.62.5 ==
== Started at 2020-05-25 01:51:58.687 ==

=> Loading problem specification...  done in 278 ms (0 seconds).
----------------------------------
Problem source: _s_
Problem target: PitchMACC
----------------------------------
=> Performing Tseitin transformation...  done in 140 ms (0 seconds).
|+| Solvers: [MaxSAT]

==================================
=> BEST solution found by MAX-SAT-SOLVER for:
Source: _s_
Target: PitchMACC
=== Security Metric ===
Joint probability of failure: 6.3E-6
[+] Failed nodes: none
Total critical nodes: 4
[+] Most likely mission-critical set (MLMCS): (SP1,0.05); (SP2,0.05); (SP3,0.05); (SP4,0.05);
[*] Metric computation time: 94 ms (0 seconds).
==================================
Solution saved in: ./view/sol.json
== LDA4CPS ended at 2020-05-25 01:51:59.237 ==
```
Go to [http://localhost:8000/viz.html](http://localhost:8000/viz.html)  
You should see the following AND/OR graph along with the MLMCS (*SP1*, *SP2*, *SP3*, *SP4*):

![Screenshot - aircraft case 2](https://github.com/mbarrere/lda4cps/blob/master/screenshots/aircraft-case2.png)

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
