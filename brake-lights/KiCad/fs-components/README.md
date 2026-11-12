# Working with the symbol, footprint, and 3d libraries

Should you have to make and edits (you will!), be sure to follow this procedure:

1. Pull the submodule, to be sure you're working on the latest version: `git submodule update`
2. Make your changes...
3. Commit _to the fs-components_ repo. You can do this with git cli by descending into the fs-components directory and using git commands from there.
4. Push! Try to do this in the minimum time possible, because if you hold changes locally and someone else commits before you, you'll need to redo your changes due to merge conflicts.

Note for maintainers: `formulaslug_drawing_sheet.kicad_wks` is a KiCad Drawing
Sheet used to format the title blocks in the schematic and pcb pages. It is
setup in the schematic and PCB of the template project.
`./formulaslug_logo.kicad_wks` is a Drawing Sheet containing only a poly-polygon
version of the FS logo, used to create the main Drawing Sheet.
