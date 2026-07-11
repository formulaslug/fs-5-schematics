# General Notes

- Clone this repository recursively (`git clone --recursive`). If you didn't, be
  sure to initialize the submodule: `git submodule init` _and_ `git submodule
update`

# Creating a project

1. Create a new branch _from main_. Name your branch in-kebab-case
2. Duplicate the template folder. Rename it _exactly the same as the branch
   name_--this is your project name.
3. Rename the files ending in `.kicad_pro`, `.kicad_sch`, and `.kicad_pcb`.
   Other files can be safely ignored. _Make sure to name these the same as the
   branch name_.
4. Create an initial commit and get to work!

# Working with the symbol, footprint, and 3d libraries

Should you have to make any edits (you will!), be sure to follow this procedure:

1. Pull the submodule, to be sure you're working on the latest version: `git submodule update`
2. Make your changes...
3. Commit _to the fs-components_ repo. You can do this with git cli by
   descending into the fs-components directory and using git commands from
   there.
4. Push! Try to do this in the minimum time possible, because if you hold
   changes locally and someone else commits before you, you'll need to redo your
   changes due to merge conflicts.
