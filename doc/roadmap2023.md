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
