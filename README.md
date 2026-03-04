# Forza Car Part Modder

A Windows tool for inspecting and modifying **Forza Motorsport 4 `carbin` files**.  
It allows you to swap, build, and reorganize car parts and subparts across LOD levels while preserving compatibility with the game engine.

The goal of the project is to make **model editing and part experimentation possible without rebuilding entire carbin structures manually**.

----------

# Overview

Forza Motorsport 4 uses a proprietary binary format (`carbin`) to store vehicle models.  
Each file contains a set of **sections (parts)**, and each section contains **subsections (subparts)** with their own geometry and metadata.

This tool provides a GUI to:

-   Inspect parts and subparts
    
-   Swap parts between carbins
    
-   Convert LOD geometry
    
-   Build new parts
    
-   Rename and reorganize existing structures
    
-   Modify transform offsets and bounds
    
-   Compare carbin contents
    

The editor is designed to be **non-destructive** whenever possible and prioritizes **in-game compatibility**.

----------

# Requirements

-   Windows
    
-   Python **3.10+**
    
-   Tkinter (included with Python)
    

Optional dependencies (experimental features):

pyglet

Install with:

    pip install pyglet


----------

# Interface Overview

The application is organized into several tabs.

 - Parts
 - Swap / Build
 - Subparts (**obsolete**)
 - Transform
 - Compare
 - Manage

Each tab serves a different stage of the workflow.

----------

# Parts Tab

The **Parts** tab is the main inspection interface.

It displays two trees:

**SOURCE carbin  
TARGET carbin**

Each entry represents a **section (part)**.

Columns show the name of the part, the LOD it belongs to, how many subparts it has, etc.

Selecting a part displays detailed metadata in the **Selection Detail** panel.

This is useful for understanding:

-   geometry pool sizes
    
-   subsection layout
    
-   LOD structure
    

----------

# Swap / Build Tab

This tab performs the **core geometry operations**.

It allows copying geometry between carbins or building new parts.

## Modes

### Template Swap (Donor LOD → Target LOD)

Converts a donor LOD level to match a target LOD layout.

Useful for:

-   bringing **race-LODX parts into showroom LOD0**
    
-   adapting LOD1 geometry for LOD0 use
    

### Same-LOD Swap

Direct replacement with no conversion.

Best for:
    
-   swapping parts between cars with identical layout
    

### Replace With Donor (Raw)

Direct binary replacement of the target section.

Use only when formats are known to match.

----------

## Safety Options

### Keep TARGET name

Preserves the original part name after replacement.

### Transform owner

Choose whether the **offset and bounds** (or position and dimensions) come from:

**Source** - useful if you want the part to keep the source positioning and size
**Target** - useful if you want the part to keep the target positioning and size

### Upconvert index size

Some LOD conversions require **16-bit indices → 32-bit indices**.

This option automatically performs that conversion.

**Use for LOD1 to LOD0 conversions!**

### Keep TARGET vertex pool

Pads donor vertex data to match the target pool size.

Useful for avoiding pool mismatch crashes.

**Use for LOD1 to LOD0 conversions!**

----------

## Apply Actions

### Replace TARGET part

Replaces the currently selected TARGET part.

### Add new part

Creates a new section at the end of the file.

This is useful when adding experimental geometry.

**New name part** is only used for **Add new part**.


## How to replace parts

- Load the Source and Target Carbin using the buttons at the top.
- Select a Source and a Target part in the Parts tab.
- Choose the Build mode and other options.
- Click Replace Target part, and overwrite the original carbin file.

If you need to replace other parts, load the target carbin file again.

----------

# Subparts Tab

**Obsolete** - Replaced by **Manage**

----------

# Transform Tab

Every section contains **transform information**:

Offset (X Y Z)  
Bounds minimum (Left)  
Bounds maximum (Right)

These values determine:

-   part placement
    
-   bounding boxes
    
-   culling behavior
    

### Load from TARGET selection

Reads transform values from the selected TARGET part.

### Copy from SOURCE selection

Copies SOURCE transform values.

### Apply to TARGET file

Writes new transform values to the carbin.

### Open 3D Preview

Provides a very simplified render of the position of the parts in real-time. Experimental.

----------

# Compare Tab

The Compare tab analyzes differences between SOURCE and TARGET carbins.

It can help identify:

-   missing parts
    
-   different LOD layouts
    
-   mismatched vertex pools
    

This is useful when preparing parts for swaps.

----------

# Manage Tab

The Manage tab provides **structure editing tools**.

It allows modifying part and subpart organization without rebuilding geometry.

Select the parts or subparts to be changed and use the commands on the side.

## Features

### Rename

You can rename parts and subparts.

It's especially useful for subparts, as you can set specific names to change properties of the subpart. For example: body defines that the specific part will be painted. Chrome makes the part Chrome. A full list is being researched.

### Duplicate

You can duplicate parts and subparts.

### Remove

You can remove parts and subparts.

### Move/Replace

You can move or replace parts and subparts, but please be aware that these overwrite the target part with the selection. There is an option to append new subparts to selected parts (through the **Allow RAW** append option), but this will often result in glitchy parts.

## Notes

- Please reload the Carbin file and click refresh after each new Save as.

----------

# LOD System

Forza uses multiple levels of detail:

LOD0  – showroom / close inspection  
LOD1  – race  
LOD2+ – distant models

Not all parts exist in all LOD levels.

The tool allows converting between LODs when possible.

----------

# Known Limitations

-   Vertex format detection is not fully automated.
    
-   Some swaps may produce **scrambled geometry** if vertex layouts differ.
    
-   Not all carbin structures are identical across vehicles.
    
-   Some parts depend on **game database references** to appear in-game.
    

----------

# Safety Notes

Always keep backups of original carbins.

Recommended workflow:

1. duplicate carbin  
2. test edits on the duplicate  
3. verify in Forza Studio  
4. test in game

----------

# Future Roadmap

The long-term goal is to make this a **complete Forza carbin editing environment**.

## Planned Improvements

### 3D preview system

A built-in viewer for:

-   part geometry
    
-   bounds
    
-   alignment
    

### Ghost mesh overlay

Display SOURCE and TARGET geometry simultaneously.

### Vertex format detection

Automatically detect vertex layouts.

### Wireframe preview

Render triangle structure instead of point clouds.

### Pivot manipulation

Move parts directly in the viewport.

### Automatic pool remapping

Prevent scrambled geometry when moving subparts.

### Database integration

Connect part editing with:

ForzaCarEditor  
SLT databases

### Part library system

Save reusable parts for later insertion.

### Multi-car batch tools

Apply part edits to multiple vehicles.

----------

# Related Tools

This project works well alongside:

-   **Forza Studio**
    
-   **ForzaCarEditor**
    
-   custom SLT database tools
    

----------

# Contributing

Contributions are welcome.

Areas that would benefit from help:

-   reverse engineering vertex formats
    
-   improving geometry conversion
    
-   implementing the preview renderer
    
-   optimizing section rebuild logic
    

----------

# Disclaimer

This tool is intended for **research and modding purposes only**.

It is not affiliated with:

Microsoft  
Turn 10 Studios  
Forza Motorsport

Use at your own risk.
