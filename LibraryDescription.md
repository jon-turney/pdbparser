# Executables #

Sources contain symeng.dll, core engine of parsing library, and sample demo program that uses this library, pdbparser.exe.
symeng.dll consists of several modules:
  * 'MSF' : contains internal MSF parsing routines. Interface: open MSF (by FileName), close MSF, read stream, get stream info, get file info.
  * 'PDB' : contains interface PDB headers parsing routines. Interface: open PDB for MSF, close PDB, find symbol by address/name, enumerate symbols, enumerate types, find PDB for image file, download PDB for image file.
  * 'DBI' : contains internal DBI parsing routines. Interface: open DBI for PDB, close DBI, load symbols, get GSI/PSI, get TPI.
  * 'SYM' : contains generic internal symbol parsing routines (core of GSI/PSI modules). Interface: open symbols for DBI, close symbols, find symbol, enumerate symbols, load symbols from symbol server, find symbols in local symbol cache.
  * 'GSI','PSI' : contains internal GSI/PSI parsing routines. Interface: open (GP)SI for SYM, close (GP)SI, get prev/next symbol, find symbol by name/address.
  * 'TPI' : contains internal TPI parsing routines. Interface: open TPI for DBI, close TPI, enumerate types, find type by ID, dump types in C header file format.
  * 'Misc' : miscallenous internal routines.

# The exported interface functions #
Main operations supported:
  * open/close PDB (by file name)
  * find PDB for image (by image base address, by image file name)
  * download PDB for image (by image base address, by image file name)
  * get PDB signature & age (unique identifier in symbol storage)
  * enumerate all types in TPI stream (type information)
  * enumerate all GSI symbols (Global Symbol Information)
  * enumerate all PSI symbols (Private Symbol Information)
  * enumerate all types and print type information in format of C header file (pdbdump analogue; detection of C-unions pending)
  * find address of symbol
  * find symbol by address
  * find prev/next symbol
  * dump symbol (from GSI/PSI)
  * dump type (from TPI)

# Library files #
  * symeng.dll - dynamically linked library
  * symeng.lib - import library
  * symengst.lib - static library
  * msf.h - declarations of MSF module exports
  * pdb.h - declarations of PDB module exports
  * dbi.h - declarations of DBI module exports
  * tpi.h - declarations of TPI module exports
  * psi.h,gsi.h - declarations of GSI/PSI modules' exports
  * sym.h - declarations of SYM module exports
  * pdb\_info.h - private header with PDB file structures