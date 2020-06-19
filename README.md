# SIM900
Elliott 900 Simulator written in F# by Andrew Herbert

Repo is set up for loading into Visual Studio

Contents:

Simulator.sln -- VS solution file
Simulator -- source, object and binary files
Demos -- Simulator scripts to run demonstrations of Elliott 900 series
software.

Using the simulator:

The simulator only works under Microsoft Windows.  Sadly the GUI is not
portable.

There are two ways to run simulations.  Either running under Visual
Studio (debugging) control or as a standalone binary.

If running under Visual Studio, load simulator.sln as a solution to
make the simulator the current solution/projecty. Start the simulator
via the Debug menu and as the first command type "DEMOS".  This will
locate and make the folder of demonstrations the working folder.  Use
the simulator ls and cd commands to move around the demo folder and
its sub-folders and the command do to run an individual demo, as in
"do demo1".

If running standalone, copy the folder simulator/bin/Release/net471 to
a convenient place with a possibly more helpful name, e.g., SIM900.
Then also copy the folder Demos into this new folder.  Running a
command shell, make the new folder current e.g., cd ...\SIM900, and
then type SIM900 to run the simulator.  Again, as with Visual Studio use
the DEMOS command to make Demos folder the current folder.
