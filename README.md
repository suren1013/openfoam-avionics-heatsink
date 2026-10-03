# OpenFOAM Avionics Heat Sink

CFD-based study of forced-convection cooling for an avionics heat sink using OpenFOAM.

This repository contains the simulation cases developed progressively from basic forced-convection validation to conjugate heat transfer, conventional heat-sink optimization, and localized hotspot thermal-management studies.

## Project Objective

The project investigates how heat-sink geometry, airflow, and non-uniform heat loading affect:

- Airflow distribution
- Convective heat transfer
- Solid conduction
- Maximum temperature
- Temperature uniformity
- Pressure drop
- Thermal resistance
- Thermo-hydraulic performance

The final goal is to develop a forced-air-cooled avionics heat-sink model that can manage localized thermal hotspots while maintaining acceptable airflow and pressure-drop requirements.

## Current Repository Structure

```text
openfoam-avionics-heatsink/
│
├── cases/
│   ├── Level1_TransverseFin/
│   │   ├── 0/
│   │   ├── constant/
│   │   └── system/
│   │
│   └── Level1_Longitudinal/
│       ├── 0/
│       ├── constant/
│       └── system/
│
└── README.md
```

## Level 1: Forced-Convection Validation

At Level 1, the fins are treated as wall boundaries.

The solid fin material is not solved internally yet. The purpose is to validate the fluid-side setup and understand airflow and temperature transport around simplified fin geometries.

### `Level1_TransverseFin`

Air approaches the fin surfaces approximately perpendicular to the fin orientation.

This case is used to understand and validate:

- OpenFOAM case structure
- Mesh generation
- Velocity boundary conditions
- Pressure solution
- Temperature transport
- No-slip walls
- Heated-wall boundary conditions
- Convective heat transfer around a simplified fin geometry

### `Level1_Longitudinal`

Air flows through channels formed between parallel fins.

This configuration is closer to a conventional plate-fin heat sink.

The case is used to study:

- Developing channel flow
- Velocity profile development
- Thermal boundary-layer development
- Flow between adjacent fins
- Pressure variation through the channel
- Temperature distribution in the airflow

## Standard OpenFOAM Case Structure

Each case follows the standard OpenFOAM directory structure.

### `0/`

Contains the initial and boundary conditions for solution variables such as:

- `U` - velocity
- `p` - pressure
- `T` - temperature

### `constant/`

Contains physical properties and mesh-related information.

Depending on the case, this may include:

- Fluid properties
- Transport properties
- Thermophysical properties
- Turbulence settings
- Mesh data
- Region properties for conjugate heat transfer

### `system/`

Contains the numerical and simulation-control files, including:

- `blockMeshDict`
- `controlDict`
- `fvSchemes`
- `fvSolution`

## Typical Workflow

Enter the required case directory and initialize the OpenFOAM environment:

```bash
source /opt/openfoam*/etc/bashrc
```

Generate the mesh:

```bash
blockMesh
```

Check the mesh:

```bash
checkMesh
```

Run the solver used by the case.

Visualize the solution using ParaView:

```bash
paraFoam
```

## Quantities of Interest

The project focuses on extracting quantities such as:

- Velocity field
- Temperature field
- Pressure field
- Inlet-to-outlet pressure drop
- Maximum temperature
- Average temperature
- Heat-transfer rate
- Thermal resistance
- Temperature non-uniformity
- Flow distribution between fin channels

These quantities will later be combined to evaluate the thermal and thermo-hydraulic performance of each design.

# Project Roadmap

## Level 1: Fluid-Side Forced-Convection Validation

Goal:

Develop and validate the basic airflow and temperature-transport model.

Main tasks:

- Build simple fin-flow geometries
- Verify boundary conditions
- Study velocity-profile development
- Study thermal boundary-layer development
- Validate pressure and temperature behaviour
- Establish a reliable OpenFOAM workflow

Current cases:

- `Level1_TransverseFin`
- `Level1_Longitudinal`

## Level 2: Conjugate Heat Transfer

Goal:

Introduce the solid heat sink and solve conduction and convection together.

Physical process:

```text
Heat source
    ↓
Heated base
    ↓
Conduction through base and fins
    ↓
Convection to airflow
```

At this level, the model will include:

- Solid base plate
- Solid fins
- Fluid region
- Solid-fluid thermal coupling
- Heat conduction through the heat sink
- Forced convection to the cooling air

Important outputs:

- Base temperature
- Fin temperature distribution
- Maximum solid temperature
- Heat-transfer rate
- Thermal resistance
- Pressure drop

## Level 3: Conventional Heat-Sink Optimization

Goal:

Create a validated conventional plate-fin heat sink and determine how its geometry affects thermal and pressure-drop performance.

Possible design variables include:

- Fin spacing
- Fin height
- Fin thickness
- Number of fins
- Base thickness
- Air velocity

The objective is not simply to maximize heat transfer.

Each configuration must be evaluated using both:

- Thermal performance
- Pressure-drop penalty

This produces a conventional baseline before introducing the final hotspot problem.

Typical performance metrics may include:

```text
Thermal resistance:

R_th = (T_max - T_inlet) / Q
```

and:

```text
Pressure drop:

Δp = p_inlet - p_outlet
```

## Level 4: Localized Hotspot Thermal Management

Goal:

Study the behaviour of the optimized conventional heat sink when the heat source is non-uniform.

Instead of applying uniform heating to the entire base, one or more localized high-heat-flux regions will represent concentrated heat generation from avionics electronics such as processors, power modules, or other high-power components.

The Level 4 study will investigate:

- Hotspot location
- Hotspot size
- Hotspot heat flux
- Multiple hotspot configurations
- Interaction between hotspot position and airflow direction
- Heat spreading through the base
- Heat conduction into nearby fins
- Maximum base temperature
- Temperature non-uniformity
- Pressure drop
- Thermal resistance

### Main Research Problem

A conventional heat sink may perform well under uniform heat loading but can still develop excessive local temperatures when heat generation is concentrated in a small region.

The research therefore focuses on the question:

> How does localized non-uniform heat generation affect the thermal and thermo-hydraulic performance of a forced-air-cooled plate-fin avionics heat sink?

A second stage may investigate:

> How can the heat-sink configuration be modified to reduce hotspot temperature without introducing an excessive pressure-drop penalty?

## Possible Hotspot-Mitigation Strategies

Geometry modification is treated as a response to the hotspot problem rather than the main research topic.

Depending on the Level 4 results, possible mitigation strategies may include:

- Local variation in fin spacing
- Additional fins near the hotspot
- Local fin-height modification
- Modified base thickness
- Non-uniform fin distribution
- Airflow redistribution
- Targeted geometry changes near high-heat-flux regions

Advanced concepts such as variable-pitch or offset fins may be investigated only if they directly contribute to hotspot mitigation and remain feasible within the project scope.

# Research Direction

The project is centered on the following overall research question:

> How can a forced-air-cooled plate-fin heat sink be designed to effectively manage localized thermal hotspots in avionics electronics while maintaining acceptable thermal resistance, pressure drop, and airflow requirements?

This changes the project from a simple fin-geometry comparison into a localized thermal-management and thermo-hydraulic optimization problem.

## Why Hotspots Matter

Electronic systems rarely generate heat perfectly uniformly.

Localized high-power components can create:

- High local temperatures
- Strong temperature gradients
- Thermal stress
- Reduced component reliability
- Uneven utilization of the heat sink

A hotspot-based study therefore provides a more realistic thermal-management problem than uniform heating alone.

# Planned Study Sequence

```text
Level 1
Fluid-side validation
        ↓
Level 2
Conjugate heat transfer
        ↓
Level 3
Conventional heat-sink optimization
        ↓
Validated baseline design
        ↓
Level 4
Localized hotspot loading
        ↓
Hotspot behaviour analysis
        ↓
Targeted hotspot-mitigation strategy
```

# Software

The project uses:

- OpenFOAM
- ParaView
- Linux / Ubuntu
- WSL where required
- Git and GitHub for version control

The current cases were developed using the OpenFOAM Foundation distribution.

# Current Status

- [x] OpenFOAM installation and environment setup
- [x] Basic OpenFOAM case structure
- [x] Basic meshing practice
- [x] Level 1 transverse-fin case
- [x] Level 1 longitudinal-fin case setup
- [ ] Complete longitudinal-flow validation
- [ ] Extract pressure and temperature metrics
- [ ] Calculate pressure drop
- [ ] Establish Level 1 validation results
- [ ] Build Level 2 conjugate heat-transfer model
- [ ] Validate solid-fluid thermal coupling
- [ ] Perform Level 3 conventional geometry study
- [ ] Select conventional baseline heat sink
- [ ] Introduce localized hotspot heat loading
- [ ] Study hotspot location, size, and intensity
- [ ] Investigate targeted hotspot-mitigation strategies
- [ ] Compare thermal and pressure-drop performance

# Repository Development

This repository is being developed progressively as the CFD model becomes more realistic.

Case definitions, geometry, numerical schemes, boundary conditions, mesh resolution, and post-processing methods may change during validation and refinement.

The priority is to establish a validated baseline before introducing optimization and hotspot-specific modifications.
