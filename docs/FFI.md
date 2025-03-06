# FFI.f90

## Purpose
The `FFI.f90` file is responsible for handling file input and output operations. It contains subroutines for opening files, reading data, and parsing input lines. The main subroutines include `FFIOpen`, `GetString`, `GetInt`, `GetReal`, `KeyWord`, `ReadLine`, `pcomp`, `isNumber`, and `ErrorMsg`.

## Main Subroutines
1. `FFIOpen()`
   - Purpose: Open input/output files and initialize variables.
   - Inputs: None
   - Outputs: None

2. `GetString(mystring)`
   - Purpose: Get a string from the input line.
   - Inputs:
     - `mystring`: String to store the input line
   - Outputs: None

3. `GetInt()`
   - Purpose: Get an integer number from the input line.
   - Inputs: None
   - Outputs:
     - `GetInt`: The integer number

4. `GetReal()`
   - Purpose: Get a real number from the input line.
   - Inputs: None
   - Outputs:
     - `GetReal`: The real number

5. `KeyWord(kw, nbkw)`
   - Purpose: Identify the keyword from the input line.
   - Inputs:
     - `kw`: Keyword set
     - `nbkw`: Number of keywords
   - Outputs:
     - `KeyWord`: Position of the input keyword

6. `ReadLine()`
   - Purpose: Read one line from the input file and parse it.
   - Inputs: None
   - Outputs:
     - `ReadLine`: Number of words in the line
     - `cmd_line`: Array of words in the line

7. `pcomp(a, b, n)`
   - Purpose: Compare character strings for a match, ignoring case differences.
   - Inputs:
     - `a`: Character string 1
     - `b`: Character string 2
     - `n`: Number of characters to compare
   - Outputs:
     - `pcomp`: True if the strings match, false otherwise

8. `isNumber(num)`
   - Purpose: Verify if a string is a numeric field.
   - Inputs:
     - `num`: String to verify
   - Outputs:
     - `isNumber`: True if the string is numeric, false otherwise

9. `ErrorMsg()`
   - Purpose: Display an error message for input errors.
   - Inputs: None
   - Outputs: None

## Parameters
1. `iord`: Input file unit number (default: 11)
2. `iomsg`: Message file unit number (default: 13)
3. `iow1`: Output file unit number (default: 14)
4. `iow2`: Output file unit number (default: 15)
5. `iow03`: Output file unit number for energy plot data (default: 101)
6. `iow04`: Output file unit number for momentum plot data (default: 102)
7. `iow05`: Output file unit number for contact force plot data (default: 103)
8. `iow10`: Output file unit number for grid data (default: 110)
9. `iow11`: Output file unit number for animation data (default: 111)
10. `iow12`: Output file unit number for animation collection data (default: 112)
11. `jobname`: Job name
12. `FileInp`: Input data file name
13. `FileOut`: Message file name
14. `fName`: File name
15. `FileCurv`: Curve data file name (TecPlot)
16. `FileAnim`: Animation data file name (TecPlot)
17. `FileAnimColl`: Animation data file name (ParaView)
18. `line_nb`: Current line number
19. `nb_word`: Number of words left in the line
20. `nb_read`: Number of words read in the line
21. `sss`: String buffer
22. `cmd_line`: Command line buffer

## Equations
1. None
