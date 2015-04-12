# Notice #

Before we start: format of _symbol record_ is described in the separate document (see SymRecordFormat).

# Global Symbol Information #
GSI stream contains GSI header followed by symbol records for all globals (global variables, interface functions).

## GSI header ##
??

## Symbol Record ##
Symbol record format is the same for all three streams and is described in SymRecordFormat.

# Public Symbol Information #
??

# Implementation #
  * pdb\_info.h : definitions of internal PDB structures, symbols, types, etc..
  * gsi.cpp : GSI/PSI parser