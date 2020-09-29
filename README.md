# Kicad Component Layout Plugin

A python plugin for KiCad to assist with script driven component layout.

## How to install

The `component_layout_plugin.py` file needs to be located into the KiCad script search path. For
example, on linux, it could go in `~/.kicad/scripting`. Once there, the script should be available
to run in KiCad under 'Tools -> External Pluging'.

## How to use

When run in pcbnew, the plugin reads information about how to position components on the board from
the file `layout.yaml` located in the project directory. The data in layout yaml allows the plugin
to change the layer (top/bottom), position, rotation, and footprint for the given module.

An example layout.yaml file:

```
    origin: [x0, y0] # Offset applied to all component locations
    components:
        R1:
            location: [x, y] # mm
            rotation: [r] # degrees
            flipped: false
            footprint:
                path: path/to/library.pretty
                name: SomeFootprint
        J1:
            ...
```

All the fields are optional for each component (e.g. leave footprint unspecified
to leave the footprint unchanged).

The footprint path is relative to the directory containing the KiCad PCB file.

Typically, I will then create a python script to generate the layout.yaml based on the needs of
the design.

The components must be instantiated in the schematic first. There are a couple caveats
to doing this:

1. The schematic has to assign a footprint, so if you override the footprint in
layout.yaml, the plugin will have to be run again after importing changes from
schematic in order to restore the footprint.
2. You have to check the "Re-associate footprints by refdes" option when performing
the update from schematic.


