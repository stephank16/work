# AGENTS.md

## What this is

A personal working repo ("daily work") with architecture diagrams (PlantUML) and
Jupyter notebooks focused on climate data infrastructure (ESGF, DKRZ, PIDs,
RAiD, CMIP6, ERA5, Globus).

## Render PlantUML diagrams

```sh
# renders all *.puml files recursively -> PNG alongside each source
./run_pssd1.sh
```

The script just runs `java -jar /home/stephan/app/plantuml.jar "**/*.puml"`.

## Directory layout

| Path | Content |
|---|---|
| `PlantUML/` | PlantUML architecture diagrams (DKRZ, ESGF, PID, catalog, FDO, dask, RAiD) |
| `PlantUML/Examples/` | ArchiMate, C4, misc examples |
| `NBs/` | Jupyter notebooks and scripts for climate data work |
| `Test/` | Scratch/test `.puml` sketches, PID prefix config, misc scratch |
| `run_pssd1.sh` | PlantUML render script |
| `README.md` | Minimal; includes a mermaid graph |

## Execution environment

- Python via conda (VS Code configured with `ms-python.python:conda`)
- PlantUML requires Java (`/home/stephan/app/plantuml.jar`)
- No package manager manifests, no test framework, no CI config
