Simulator for Elliott 900 Series Computers by Andrew Herbert

Includes large suite of demonstration programs.

Written in F#.  Only runs on Microsoft Windows.

Simulator.sln  -- Microsoft Visual Studio "solution" file for the Simulator.

Simulator      -- tree containing source, object and binary files.

Demos          -- simulator scripts to demonstrate Elliott 900 series software including compilers 
                  for langauges such as Algol 60 and FORTRAN.
                  
Manual         -- directory containing Word and PDF versions of Simulator manual.

README.md      -- this file.

LICENSE.TXT    -- MIT license for use of the simulator software.

To run the simulator under Visual Studio simply open the Simulator.sln file.  Build and start the system.  
As the first command, type "DEMOS" to make the demonstration tree the working directory.  Then change to 
one of the sub-directories by e.g., "CD 903ALGOL" and run a demonstration  by typing e.g., "DO DEMO3".

To run the simulator standalone explore the Simulator directory and find Simulator\Bin\Release\net471.
Copy this folder to a convenient location.  Copy the Demos folder into this new folder.  Make the new folder
current and run SIM900.exe.  Then as with Visual Studio, start with the command "DEMOS", etc., etc.
