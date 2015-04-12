# Two versions of header #

DBI header located in the beginning of the DBI stream has two versions.
Old version (DBIHdr) is used in PDB Format v. <= 4.0
New version (NewDBIHdr) is used in PDB Format v. 7.0 (current version).

## Old header ##
```
struct DBIHdr {
/* 000 */ WORD snGSSyms;   // GSI stream #
/* 002 */ WORD snPSSyms;   // PSI stream #
/* 004 */ WORD snSymRecs;  // Sym stream # (unsorted array of all symbol records)
/* 006 */ WORD reserved;   // padding
/* 008 */ LONG cbGpModi;   // (unknown)
/* 00c */ LONG cbSC;       // (unknown)
/* 010 */ LONG cbSecMap;   // (unknown)
/* 014 */ LONG cbFileInfo; // (unknown)
/* 018 */ // Total bytes
};
```

## New header ##
```
struct NewDBIHdr {
/* 000 */ DWORD verSignature;     // DBI signature (always ??)
/* 004 */ DWORD verHdr;           // header version
/* 008 */ DWORD age;              // PDB age
/* 00c */ WORD snGSSyms;          // GSI stream #
/* 00e */ WORD usVerPdbDllMajMin; // PDB's DLL version (Major.Minor)
/* 010 */ WORD snPSSyms;          // PSI stream #
/* 012 */ WORD usVerPdbDllBuild;  // PDB's DLL build number
/* 014 */ union {
/* 014 */   WORD snSymRecs;       // Sym stream #
/* 014 */   DWORD ulunusedPad2;   // padding to DWORD
/* 014 */ };
/* 018 */ LONG cbGpModi;          // (unknown)
/* 01c */ LONG cbSC;              // (unknown)
/* 020 */ LONG cbSecMap;
/* 024 */ LONG cbFileInfo;
/* 028 */ LONG cbTSMap;
/* 02c */ DWORD iMFC;
/* 030 */ LONG cbDbgHdr;
/* 034 */ LONG cbECInfo;
/* 038 */ WORD wFlags;
/* 03a */ WORD wMachine;
/* 03c */ DWORD rgulReserved [1];
/* 040 */ // Total bytes
};
```

## Header fields ##

# Symbol loading algorythm #
Sample algorythm for dumping all symbols with their types follows:
  1. Read DBI stream
  1. Parse DBI header : retrieve GSI/PSI stream numbers, retrieve Sym stream number (unsorted list of all referenced symbol records).
  1. Remember pointers to GSI/PSI/Sym. (GSI can be simply enumerated, PSI supports hash-based search (AFAIR), Sym can be used for easy searching symbol by address. Also, PDB has an additional hash lookup table for fast symbol-by-address search).
  1. Load TPI
  1. Enumerate all Sym records
  1. 
    * for each Sym record find appropriate type descriptor (or predefined type)
    * display type name with symbol name