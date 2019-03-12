# PowerMarieEdu

The software package provides the solution of the AC and DC power flow, the non-linear and DC state estimation, as well as the state estimation with PMUs, with built-in measurements generator.

## Modules

There are three basic modules:
   - **power flow** - using the executive m-file *power_flow.m* runs the AC and DC power flow;
   - **state estimation** - using the executive m-file *state_estimation.m* runs the non-linear, DC and PMU state estimation;
   - **state estimation with built-in measurments generator** - using the executive m-file *power_estimation.m* runs the AC power flow, generates a set of measurements and continues with the state estimation.
   
Note: The state estimation with PMUs is currently not available.

## Power Flow
### Input Data

The input data are located in the *data_power_grid* directory, as the mat-file with the struct variable *data*. The power system data is given in the variable *data.system*, with variables *bus*, *generator*, *line*, *inTransformer*, *shiftTransformer*, and *baseMVA*.  The minimum amount of information with each instance of the data structure to run the module requires *bus* and one of the variables *line*, *inTransformer* and *shiftTransformer*. In the following, we describe the structure of the variable *data.system*.
   - **data.system.bus** with columns: (1) bus number; (2) bus type; (3) initial voltage magnitude; (4) initial voltage angle; (5) load real power injection; (6) load reactive power injection; (7) conductance of the shunt element; (8) susceptance of the shunt element; (9) minimum voltage magnitude; (10) maximum voltage magnitude 
   - **data.system.generator** with columns: (1) bus number; (2) generator real power injection; (3) generator reactive power injection; (4) minimum reactive power injection; (5) maximum reactive power injection; (6) voltage magnitude; (7) on/off status
   - **data.system.line** with columns: (1) from bus number; (2) to bus number; (3) transmission line resistance; (4) transmission line reactance; (5) total transmission line charging susceptance; (6) on/off status 
   - **data.system.inTransformer** with columns: (1) from bus number; (2) to bus number; (3) in-phase transformer resistance; (4) in-phase transformer reactance; (5) total in-phase transformer charging susceptance; (6) turns ratio (7) on/off status
   - **data.system.shiftTransformer** with columns: (1) from bus number; (2) to bus number; (3) phase-shifting transformer resistance; (4) phase-shifting transformer reactance; (5) total phase-shifting charging susceptance; (6) turns ratio; (7) shift angle; (8) on/off status 
   - **data.system.baseMVA**: system base power

Note that all quantities related to the power always should be given in (MVA), (MVAr) or (MW), magnitudes are in (p.u.), while angles should be given in (degree), conductance, susceptance, resistance, and reactance are also in (p.u.).

### User Options
<p align="center">
<img src="/doc/figures/power_flow_chart.png" scale="1">
</p>

The power flow user options should be defined using variable *flow*, the following form:
  1. **flow.grid**: name of the mat-file that contains power system data
      * example: flow.grid = 'ieee300_411';
  2. **flow.module**: AC or DC power flow
      * flow.module = 1 - AC power flow
      * flow.module = 2 - DC power flow
  3. **flow.reactive** and **flow.voltage**: forces reactive power and voltage magnitude constraints
      * flow.reactive = 1 - on
      * flow.voltage = 1 - on      
  4. **flow.bus** and **flow.branch**: display
      * flow.bus = 1 - on      
      * flow.branch = 1 - on        
   5. **flow.save**: write display data to a text file
      * flow.save = 1 - on        
      
## State Estimation
### Input Data
The input data are located in the *data_power_grid* directory, as the mat-file with the struct variable *data*. The state estimation module, except measurement data, requires quantities that describes the physical structure of the power system, and those are located in variable *data.system*.

Measurement data is given in variables *data.legacy* and *data.pmu*, with variables *flow*, *current*, *injection*, *voltage* for legacy measurements, and *current*, *voltage* for phasor measurement units (PMUs). The minimum amount of information with each instance of the data structure to run the module requires one of the types of measurements (e.g., *data.legacy.flow*) . In the following, we describe the structure of the variables *data.legacy* and *data.pmu*.
  - **data.legacy.flow** with columns: (1) from bus number; (2) to bus number; (3) active power flow measurement; (4) active power flow variance; (5) on/off status; (6) reactive power flow measurement; (7) reactive power flow variance; (8) on/off status; (9) active power flow exact value (optional); (10) reactive power flow exact value (optional) 
  - **data.legacy.current** with columns: (1) from bus number; (2) to bus number; (3) line current magnitude measurement; (4) line current magnitude variance; (5) on/off status; (6) line current magnitude exact value (optional)
  - **data.legacy.injection** with columns: (1) bus number; (2) active power injection measurement; (3) active power injection variance; (4) on/off status; (5) reactive power injection measurement; (6) reactive power injection variance; (7) on/off status; (8) active power injection exact value (optional); (9) reactive power injection exact value (optional)   
  - **data.legacy.voltage** with columns: (1) bus number; (2) bus voltage magnitude measurement; (3) bus voltage magnitude variance; (4) on/off status; (6) bus voltage magnitude exact value (optional) 
  - **data.pmu.current** with columns: (1) from bus number; (2) to bus number; (3) line current magnitude measurement; (4) line current magnitude variance; (5) on/off status; (6) line current angle measurement; (7) line current angle variance; (8) on/off status; (9) line current magnitude exact value (optional); (10) line current angle exact value (optional)
  - **data.pmu.voltage** with columns: (1) bus number; (2) bus voltage magnitude measurement; (3) bus voltage magnitude variance; (4) on/off status; (5) bus voltage angle measurement; (6) bus voltage angle variance; (7) on/off status; (8) bus voltage magnitude exact value (optional); (9) bus voltage angle exact value (optional).
  
Note that all quantities should be given in (p.u.) or (radian). 

### User Options
<p align="center">
<img src="/doc/figures/state_estimation_chart.png" scale="1">
</p>
