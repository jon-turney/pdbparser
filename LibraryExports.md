# Miscellaneous #
  * `halloc`, `hfree` - allocate/free from the heap
  * `hCheckLeaks` - check for memory leaks in halloc/hfree
  * `xstrdup` - make duplicate for string
  * `xstrstrskip` - find occurrence of substring in a string and skip substring length
  * `xstrreplacechar` - replace ch1 with ch2 in a string
  * `HashPbCb` - calculate hash for symbol name (used in GSI/PSI/SYM)
  * `MapExistingFileA/W` - ANSI/Unicode versions for `MapExistingFile` function - easy interface for `CreateFile`+`CreateFileMapping`+`MapViewOfFile`
  * `RtlGetModuleBase` - find image base for supplied pointer within image

# MSF exports #
  * `MsfOpen` - open MSF container
  * `MsfClose` - close MSF container
  * `MsfLoadStream` - load stream from MSF file
  * `MsfReferenceStream` - load stream + remember its data pointer and content length
  * `MsfSizeOfStream` - calculate stream content length. Always a multiple of page size.
  * `MsfDereferenceStream` - decrement stream reference count. When reference count reaches zero, stream is unloaded.
  * `MsfGetStreamPointers` - read page numbers of stream content. Used internally in `MsfLoadStream` to load stream pages.

# TPI exports #
  * `TPILoadTypeInfo` - load TPI stream from PDB
  * `TPILookupTypeRecord` - find type record by type id
  * `TPILookupTypeName` - find type name by type id
  * `TPIDumpTypes` - generate C header file-style output for TPI dump
  * `TPIFreeTypeInfo` - free TPI stream loaded by `TPILoadTypeInfo`

# SYM exports #
  * `SYMLoadSymbols` - load symbol information for PDB
  * `SYMUnloadSymbols` - unload symbol information for PDB
  * `SYMGetSymFromAddr` - get symbol record by symbol's address (RVA, of course)
  * `SYMFreeSymbol` - free symbol record retrieved by `SYMGetSymFromAddr`
  * `SYMDumpSymbols` - dump symbol information (for debugging purposes)
  * `SYMNextSym` - get next symbol record

# GSI exports #
  * `GSINearestSym` - find nearest symbol for supplied RVA
  * `GSIFindExact` - find symbol by exact address
  * `GSIEnumAll` - enumerate all GSI symbols
  * `SYMByName` - find symbol by name
  * `GSIByName` - find GSI symbol by name

# Sym engine #
  * `SymSetSymbolPath` - set path for local symbol cache
  * `SymFindLoadedSymbolsForImage` - find PDB in local symbol storage for the specified image
  * `SymLoadSymbolsForImage` - load PDB for image file (compares GUID Signature and Age from IMAGE\_DEBUG\_DIRECTORY and PDB header)
  * `SymUnloadSymbols` - unload all symbols for PDB
  * `SymUnloadAll` - ??
  * `SymFromAddress` - find symbol record by symbol address
  * `SymFreeSymbol` - free symbol record retrieved by `SymFromAddress`
  * `SymNextSym` - get next symbol record
  * `SymDownloadFromServerPdb` - download PDB from symbol server (identified by unique GUID Signature-PDB Age pair)
  * `SymDecompressPdb` - decompress CAB-packed PDB file (typically downloaded from symbol server: symbol server keeps all PDBs compressed)
  * `SymDownloadFromServerForImage` - search PDB for the specified image in symbol server (by unique Signature-Age pair)
  * `SymSetPrintfCallout` - set address of printf-like function called for printing debug output of Sym engine
  * `SymLoadSymbolsforImageFile` - automatically search PDB for image supplied (first, look in local symbol store, then look in symbol server)

# PDB exports #
  * `PdbOpen` - open PDB (by name)
  * `PdbClose` - close PDB
  * `PdbOpenValidate` - open PDB if it matches GUID Signature and Age supplied
  * `PdbOpenForImage` - open PDB if it matches image supplied
  * `PdbOpenForImageFile` - open PDB if it matches image supplied by image file
  * `PdbGetImageSignatureAndPath` - get PDB path and GUID signature/Age for the PE image supplied (typically, by image base address)
  * `PdbFindMatchingPdbForImage` - find appropriate PDB file for the image supplied
  * `PdbFindMatchingPdbForImageFile` - find appropriate PDB file for the image file supplied
  * `PdbGetLastError` - get last PDB error
  * `PdbSetLastError` - set last PDB error
  * `PdbGetErrorDescription` - get string description for the error code supplied

# Temporarily exported internals #
  * `SympFindBestOffsetInMatchingSection` - find best matching section in PE image for the specified RVA