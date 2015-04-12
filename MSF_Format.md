# Main principles #

MSF file consists of a set of streams. Each stream has the following properties:
  * ID (integer, 0-based. for example, Stream #1 always contains PDB headers)
  * Content (data pointer + length (`= NumberOfPages * MSF->PageSize`))
  * Array of page numbers
  * Number of pages

# MSF header #
MSF header (see MSF\_HDR in symeng\msf.cpp) has the following layout:
| **Offset** | **Size** | **Comment** |
|:-----------|:---------|:------------|
| 0x00 (0) | 32 | Signature `#define MSF_SIGNATURE_700 "Microsoft C/C++ MSF 7.00\r\n\032DS\0\0"` |
| 0x20 (32) | DWORD | `PageSize`, in bytes. File size is always a multiple of page size |
| 0x24 (36) | DWORD | FPM Page# (Free Page Map). Destiny: unknown (field ignored) |
| 0x28 (40) | DWORD | Page Count. File size always equals `PageSize*PageCount` |
| 0x2c (44) | DWORD | Root directory size, in bytes |
| 0x30 (48) | DWORD | reserved (not used, ignored) |
| 0x34 (52) | array of 0x49 (73) DWORD's | Root directory pointers (page numbers containing root directory) |

Total length: 0x158 (344) bytes.

# Maximum size of MSF container #
Hardware restrictions on maximum container size are almost absent.
Maximum number of pages for root directory, as we can see, is 0x49. Root directory can hold lots of streams with big content length. Also, page size can be as big as we wish (and 32 bits can hold).
In practice, the particular implementation of PDB library can add its own restrictions on maximum page size, peak page count or maximum container length, etc..
So, MSF containers can hold lots of data.

# Root directory #
Root directory contains array of stream descriptors. Root directory is followed by free page map (complete layout is unknown, ignored).
First DWORD of root directory is the number of streams. Number of streams is followed by array of stream sizes (in pages; number of elements == stream count).
Stream size array is followed by array of stream pointers (list of page numbers of pages occupied by the stream). Number of pages in stream pointers == stream page count.
Number of streams in outer array == stream count.

# Stream types #
| **Stream#**   | **Description** |
|:--------------|:----------------|
| 0         | Root directory (copy) |
| 1         | PDB headers (see [PDBHeadersFormat](PDBHeadersFormat.md)) |
| 2         | TPI (type info) (see [TPIFormat](TPIFormat.md)) |
| 3         | DBI (debug info) (see [DBIHeaderFormat](DBIHeaderFormat.md)) |
| 5         | FPO (frame pointers omission info) (see [FPOEntriesFormat](FPOEntriesFormat.md)) |
| variable  | GSI (global symbol information) (see [GSIPSIFormat](GSIPSIFormat.md)) |
| variable  | PSI (private symbol information) (see [GSIPSIFormat](GSIPSIFormat.md)) |

# Misc #
Calculate number of pages spanned by size-length bytes: `#define STREAM_SPAN_PAGES(msf,size) (ALIGN_UP(size,msf->hdr->dwPageSize)/msf->hdr->dwPageSize)`
Other pages of the MSF contains stream data (type depends on a stream).