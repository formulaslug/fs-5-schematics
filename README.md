# General Notes

- Clone this repository recursively (`git clone --recursive`). If you didn't, be
  sure to initialize the submodule: `git submodule init` _and_ `git submodule
update`

# Creating a project

1. Install our Template Project to your local computer.
   - Kicad requires templates to be installed locally, but our template is
     stored in the `template` folder in this repo, so you'll need to copy the
     `template` folder to where Kicad expects it. To see where that is, press
     Preferences -> Configure Paths, and look for the value of
     `KICAD_USER_TEMPLATE_DIR`. Copy the `template` folder there.
2. Create a new branch _from main_. Name your branch in-kebab-case
3. Create a new Kicad project from template. Under "User Templates" you should
   now see our Formula Slug template listed! Navigate to `fs-5-schematics` in
   the dialog and name your project the same as your branch name
   (in-kebab-case).
4. In the Schematic Editor, open File -> Schematic Setup -> Text Variables, and
   fill in the info for your board.
5. Create an initial commit and get to work!

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
