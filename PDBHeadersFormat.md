# Stream content #

PDB stream (for PDB Format v7.0) contains the header with the following fields:
  * Offset 0x00  : `DWORD impv`: not used
  * Offset 0x04  : `DWORD sig` : time stamp-based old-style signature (not used)
  * Offset 0x08  : `DWORD age` : PDB file age (number of PDB recompilations since PDB was created or completely rebuilt)
  * Offset 0x0c  : `GUID sig70` : GUID signature (new-style).
Total length: 0x10 (16) bytes.

Among with Age used as unique PDB identifier in a set of PDB with the same names. Used to distinguish PDBs for different versions of the same file in symbol server and symbol store.

# Symbol store FS layout #
Files in symbol store are saved in the path based on the following template:
`%PathToSymbolStore%\filename.exe\SIGNATUREAGE\filename.pdb` or filename.pd_(CAB-compressed PDB)
Where SIGNATUREAGE is a string concatenated from GUID signature and PDB age (without spaces or other delimeters).
For example, assume we got PDB with full path B:\Symbols\win32k.pdb\16B8ACEDB77D4682A3C71E78E31A81152\win32k.pdb.
Symbol store base is B:\Symbols
Signature is 16B8ACEDB77D4682A3C71E78E31A8115
PDB age is 2 (note: without leading zeroes, although age is a DWORD)_

See also: `PdbGetImageSignatureAndPath` (retrieves PDB file path and signature-age string)
`SymDownloadFromServerPdb` (downloads PDB from symbol server and saves in local symbol store; arguments: PDB signature (string), symbol server URL, local symbol store path)

Sample HTTP request sent to symbol server:

```
GET /win32k.pdb/16B8ACEDB77D4682A3C71E78E31A81152/win32k.pd_ HTTP/1.1
User-Agent: Microsoft-Symbol-Server/6.6.0007.5
Host: msdl.microsoft.com
Connection: keep-alive
Cache-Control: no-cache
Cookie: A=I&I=AxUFAAAAAAA/BwAAiyuzHeFC9Wr/2d6GgxRHGg!!&M=1; MC1=GUID=9cd0ed4ebbb93448869ee604aac421da&HASH=4eed&LV=20102&V=3
```

# Symbol server FS layout #
`http://symbol.server/filename.pdb/SIGNATUREAGE/filename.pdb` or `filename.pd_` (compressed).
SIGNATUREAGE part is the same as the corresponding part of PDB path from local symbol store.