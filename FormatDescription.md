# Outer format: MSF container #

The outer format of PDB file is a MSF (Multi-Stream File, see MSF\_Format).
Data is contained in so called 'streams' (numbered data streams).
Also, MSF container is divided in 'pages' (blocks of fixed size, page size resides in MSF header). Size of MSF container is always a multiple of PageSize (and equals `PageSize * PageCount`).
First page contains MSF file header (see MSF\_Format for detailed description), followed by **Root Directory** (array of stream descriptors; defines where the particular stream exists within the MSF). Root Directory contains number of streams, stream page counts and stream pointers (list of page numbers occupied by the stream).
Root Directory is usually followed by stream data. Stream pages can be placed randomly (e.g. Stream #0 can consist of Pages# 1,4-9,15, but Stream #1 can consist of Pages# 2-3,10-14, for example).
When parsing MSF container and reading stream content, parser enumerates all page numbers in StreamPointers and for each page# reads stream pages and places them in the proper order.
'cause stream may be fragmented, MSF container can't be simply mapped for manipulating pointers within this mapped view.
For reading stream contents, memory block of stream size should be allocated and there pages have to be read one by one and consequently written in this block.
Pointer to the beginning of the block is returned as 'stream data pointer' (of course, should be freed when is not yet necessary).

# MSF Container Internals #

## PDB stream ##
PDB stream contains GUID signature and PDB age for identification it in a symbol store or symbol server (which is actually a symbol store, accessible from the Web with compression-on-the-fly option).

## TPI stream ##
Contains Type Information (array of type descriptors). It includes type of function or method arguments, their return values, global structures, prototypes or classes, etc..
Also, type of structure field is also included in TPI. So, each structure includes one entry per the structure itself and one optional entry per field for describing it's field.
There is a predefined type list (`int,char,unsigned,void*,...` - all integer,real and other types, pointers to it, double-pointers to it (`type**`), etc..
List of predefined types can be found here: ListOfPredefinedTypes.

TPI info is a list of descriptors of compex types used in the image. It could be structure, pointer to a (for instance, `struct _KAPC *`), union, etc..
Each entry has an integer ID - **Type ID**. Types in the TPI are identified by this TypeID.
For example, TPI module exports function to find type descriptor by type id / find type name by type id.

Struct/class/union type info contains names, offsets and sizes, type IDs of each field (yes, complex type of each field is described in the separate type descriptor).

Also, TPI module exports a routine to format TPI dump in C header style (in the ideal: generates compileable header file; current version sometimes 'swallows' union members).

_**TODO**: Improve C header file generation in TPI dump: unions are not processed correctly.
The problem is in the absence of difference between struct or union (or their combination) in type descriptor list, so looking at the particular struct/union descriptor, parser can't tell the union descriptor from struct descriptor or from some their combination._

Symbols for variables has a separate field for type ID of this variable (can be even one of predefined types or type ID in TPI list).