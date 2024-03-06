## NativeIFC addon for FreeCAD

![FreeCAD screenshot](doc/images/main.jpg)

This project implements [NativeIFC](https://github.com/brunopostle/ifcmerge/blob/main/docs/whitepaper.rst) concept into [FreeCAD](https://freecad.org). It allows FreeCAD users to open, manipulate and create [IFC](https://en.wikipedia.org/wiki/Industry_Foundation_Classes) files natively in FreeCAD.

Although FreeCAD already supports opening and saving IFC files through the [Arch workbench](https://wiki.freecad.org/Arch_Workbench), it does so, like most other BIM applications, by translating to and from the IFC file format and FreeCAD's internal data model. This means two translations, one when opening and another one when saving, which cause a) data loss, and b) a complete rewrite of the file, which turns it unsuitable for [version control systems](https://en.wikipedia.org/wiki/Version_control) like [Git](https://en.wikipedia.org/wiki/Git).

NativeIFC means that the IFC file is itself the data structure in FreeCAD. When opening an IFC file in FreeCAD, what you see on the screen is a direct rendering of the contents of the IFC file. Anything you modify will directly modify the IFC file. If you open a file, modify one element and save the file, the only thing that will have changed in the file is the line concerning that element. The rest of the file will be 100% identical to how it was before you saved. No data loss, and very identificable changes.

The Native IFC idea itself comes from this [paper by Bruno Postle](https://github.com/brunopostle/ifcmerge/blob/main/docs/whitepaper.rst).

This project is heavily inspired by [BlenderBIM](https://blenderbim.org) and tries as much as possible to use the same concepts and reuse the code. The final goal is to offer in FreeCAD the same level of functionality and performance found in BlenderBIM, and to upstream as much as possible to [IfcOpenShell](https://ifcopenshell.org), the common IFC engine used by both applications.

The roadmap below will show you an overview of the current state of the implementation. Check the [documentation](doc/README.md) to learn how to use this addon, and I also write regular updates on this project at https://yorik.uncreated.net/blog/nativeifc

### Roadmap

#### 1. Get a working concept up

* [x] Write an importer that allows an initial import of an IFC file into FreeCAD
* [x] Write a custom parametric FreeCAD object that represents an IFC document in the FreeCAD tree
* [x] Do an initial geometry import
* [x] Write a custom parametric FreeCAD object that represents in IFC product in the FreeCAD tree
* [x] Reveal the document structure in the FreeCAD tree
* [x] Allow an object shape to be built automatically from its children
* [x] Allow to expand a document (reveal its children)
* [x] Use group extension
* [x] Support colors

#### 2. Allow basic editing

* [x] Use enums in enum-based properties
* [ ] ~~Fetch attribute documentation~~ canceled for now because it yields too much text
* [x] Fetch context-dependent IFC types
* [x] Find a way to not store the whole enum in the file
* [x] Add progress feedback
* [x] Allow to change an attribute of an object
* [x] Allow to manually save the linked IFC file
* [x] Implement mesh mode
* [x] Allow different storage strategies (full shape or only coin representation)
* [x] Write a hook system that allows FreeCAD to save the IFC document
* [x] Test (and solve!) what happens when opening a NativeIFC file in vanilla FreeCAD
* [x] Add a shape caching system
* [x] Tie the shape caching to the corresponding IFC document
* [x] Allow to change the class of an object
* [x] Allow late loading/rebuilding of coin representation

#### 3. Allow adding and removing objects

* [x] Allow different import strategies (full model, only building structure...)
* [x] Allow to create an IFC document without an existing IFC file
* [x] Allow to add building structure (building, storey...)
* [x] Allow to add a simple generic IFC product
* [x] Allow to delete objects
* [x] Allow to hide children of an object
* [ ] ~~Tie all of the above to BIM commands~~ cancelled because the only one necesary is the Project tool

#### 4. Allow advanced editing

* [x] Allow to edit placements
* [x] Define a strategy for expanding non-IfcProduct elements
* [ ] ~~Expand attributes~~ cancelled for now because only non-link attributes need to be shown so far
* [x] Expand materials
* [x] Expand properties
* [x] Allow to regroup elements
* [x] Handle drag/drop
* [x] Allow to change the IFC schema
* [x] Support layers
* [x] Support groups

#### 5. Full workflow (creation and edition) of different tools

* [x] Support Arch walls
* [x] Support Arch structures
* [ ] Support Arch objects with subtractions and additions
* [x] Support Arch windows
* [ ] Support custom openings and protrusions
* [ ] Support Draft 2D entitties
* [ ] Support Draft dimensions
* [x] Support Part extrusions
* [ ] Support Part booleans

#### Additionals

- [x] Implement single document mode
* [ ] Handle undo/redo
* [ ] ~~Add this to the addons manager~~ cancelled because it will be merged with BIM
* [ ] Add to BIM WB dependencies/ reorganize addon
* [ ] Verify and adapt 2D view generation workflow
* [ ] Verify and adapt quantifying workflow
* [ ] Verify and adapt cost/surveying workflow
* [ ] Document everything
* [ ] Upstream all pure ifcopenshell functionality to ifcopenshell.utils
* [ ] Transfer Arch exportIFC.getRepresentation() functionality to ifcopenshell.api.geometry

#### Edit mode

- [ ] Common system: Geometry properties are created on entering edit mode, on-screen trackers system
* [ ] Edit single-line axis endpoints for walls
* [ ] Edit single-line axis endpoints for extruded profiles
* [ ] Edit rectangle widths
* [ ] Edit non-axis extrusion lengths
* [ ] Edit polyline profiles

#### Further goals

* [ ] Support Types
* [ ] Support quantity sets
* [ ] Support mapped geometry
* [ ] Wall joining system

#### Questions to solve

* Mixed or single document mode must be clearly choosable and identified by the user:
  * [x] When opening an IFC file: Option in the import dialog to choose between Single/Hybrid
  * [x] When opening a FreeCAD file: Implicit - The file carries the mode already
  * [x] When creating a FreeCAD file: Dialog pops up to choose between Single/Hybrid
  * [ ] When inserting an IFC file:
    * If the active file has objects:
      * If the active file is singledoc:
        * Pop up dialog to ask between Merge/Hybrid
      * Else: Hybrid
    * Else:
      * Hybrid
* Create example workflows:
  * [ ] I am opening an existing IFC file and I want to modify/add/remove some of its contents: Opening in singledoc mode, modifying, saving.
  * [ ] I am opening an IFC file and I want to include additional IFC files in it: Opening in singledoc mode, insert, choose if merging or not
    * [ ] If merging, merge additional IFC
    * [ ] If not, creating a doc object. Any changes to that doc object must take precedence over host doc
  * [ ] I am creating an IFC file from scratch: Creating a file, choose if singledoc
  * [ ] I am creating a FreeCAD file from scratch, not only IFC: Creating a file, choose no singledoc
  * [ ] I am working on a hybrid FreeCAD file containing a BIM model. I want to export the IFC data consistently while still being able to modify the BIM objects with normal tools
  * [ ] I am working on an IFC file and I want to produce 2D documents out of it, I want to benefit of the full FreeCAD 2D capabilities while saving the result as IFC data

### Documentation

* [Installing](doc/installation.md)
* [Usage](doc/README.md)
* [IfcOpenShell code examples](doc/code_examples.md)
* [IfcOpenShell github](https://github.com/IfcOpenShell/IfcOpenShell)
* [IfcOpenShell docs](https://blenderbim.org/docs-python/ifcopenshell.html)
* [BlenderBIM docs](https://blenderbim.org/docs/)
* [IfcOpenShell matrix structure](https://github.com/IfcOpenShell/IfcOpenShell/issues/1440)
* [IfcOpenShell to FreeCAD matrix conversion](https://pythoncvc.net/?cat=203)

### Performance

Tests generated by [auto tests](ifc_performance_test.py) on a AMD Ryzen 9 5900HX - 2024.01.27

| File | File size | Import time (coin) | Import time (shape) | BlenderBIM |
| ---- | --------- | ------------------- | ------------------ | ---------- |
| IfcOpenHouse_IFC4.ifc | 0.11 Mb | 00:00 | 00:05 | 00:00 |
| FZK_haus.ifc | 2.45 Mb | 00:01 | 00:02 | 00:01 |
| schultz_residence.ifc | 21.43 Mb | 00:05 | 00:13 | 00:04 |
| nineteen_plots.ifc | 4.33 Mb | 00:11 | 00:32 | 00:05 |
| schependomlaan.ifc | 47.0 Mb | 00:11 | 00:20 | 00:05 |
| king_arch.ifc | 24.98 Mb | 00:35 | Timed out | 00:14 |
| king_arch_full.ifc | 147.96 Mb | 01:39 | Timed out | 00:36 |

### Sponsors

This project is sponsored by:

[![](doc/images/otfn-logo.png)](https://opentoolchain-foundation.org/) <div style="width: 60px;"></div> [![](doc/images/ngi0-logo.png)](https://nlnet.nl/project/FreeCAD-IFC/)
