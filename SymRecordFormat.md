# Lots of symbols! #
Symbol record is actually a union of lots of typed symbol record for a particular symbol type.
Look at this:
```
typedef union _SYM
{
	ALIGNSYM Sym;
	ANNOTATIONSYM Annotation;
	ATTRMANYREGSYM AttrManyReg;
	ATTRMANYREGSYM2 AttrManyReg2;
	ATTRREGREL AttrRegRel;
	ATTRREGSYM AttrReg;
	ATTRSLOTSYM AttrSlot;
	BLOCKSYM Block;
	BLOCKSYM16 Block16;
	BLOCKSYM32 Block32;
	BPRELSYM16 BpRel16;
	BPRELSYM32 BpRel32;
	BPRELSYM32_16t BpRel32_16t;
	CEXMSYM16 Cexm16;
	CEXMSYM32 Cexm32;
	CFLAGSYM CFlag;
	COMPILESYM Compile;
	CONSTSYM Const;
	CONSTSYM_16t Const_16t;
	DATASYM16 Data16;
	DATASYM32 Data32;
	ENTRYTHISSYM EntryThis;
	FRAMEPROCSYM FrameProc;
	FRAMERELSYM FrameRel;
	LABELSYM16 Label16;
	LABELSYM32 Label32;
	MANPROCSYM ManProc;
	MANPROCSYMMIPS ManProcMips;
	MANTYPREF ManTypRef;
	MANYREGSYM_16t ManyReg_16t;
	MANYREGSYM ManyReg;
	MANYREGSYM2 ManyReg2;
	OBJNAMESYM ObjName;
	OEMSYMBOL Oem;
	PROCSYM16 Proc16;
	PROCSYM32 Proc32;
	PROCSYM32_16t Proc32_16t;
	PROCSYMIA64 ProcIA64;
	PROCSYMMIPS ProcMips;
	PROCSYMMIPS_16t ProcMips_16t;
	PUBSYM32 Pub32;
	REFSYM Ref;
	REFSYM2 Ref2;
	REGREL16 RegRel16;
	REGREL32_16t RegRel32_16t;
	REGREL32 RegRel32;
	REGSYM Reg;
	REGSYM_16t Reg_16t;
	RETURNSYM Return;
	SEARCHSYM Search;
	SLINK32 Slink32;
	SLOTSYM32 Slot32;
	SYMTYPE SymType;
	THREADSYM32_16t Thread_16t;
	THUNKSYM Thunk;
	THUNKSYM16 Thunk16;
	THUNKSYM32 Thunk32;
	TRAMPOLINESYM Trampoline;
	UDTSYM Udt;
	UDTSYM_16t Udt_16t;
	UNAMESPACE UNameSpace;
	VPATHSYM16 VPath16;
	VPATHSYM32 VPath32;
	VPATHSYM32_16t VPath32_16t;
	WITHSYM16 With16;
	WITHSYM32 With32;
} SYM, *PSYM;
```

When you have PSYM pointer, note, that this pointer may refer to one of tens symrec types :)

## Symbol types ##
(only supported types are documented)

  * **S\_PUB32** (PUBSYM32) : public symbol, 32-bit offset; options: flags(code/function/managed/MSIL) (for example, `public function _WinMain@16 = 0002:00001FD0`)
  * **S\_CONSTANT** (CONSTSYM) : constant (for example, `enum IVTYPE::IT_TYPE1 = 5`) NB: win\_pdbx has an error in the header file where CONSTSYM is declared. Instead of 'stretching' value from 16- to 32-bit, author stretched type index to 32 bits:) Of course, definition of first two fields of CONSTSYM should look like this: `WORD typind; DWORD value; BYTE name [];`
  * **S\_UDT** (UDTSYM) : User-Defined Type (via **typedef**); what was the reason for inclusion of typedef in the list of SYMBOL TYPES?! It should be in TPI list instead. (Yes, it's duplicated in TPI as a type).
  * **S\_LPROCREF**,**S\_PROCREF** (REFSYM2) : local/global reference to a procedure; options: module:symbol, 16 bits for module index, 32 bits for symrec-offset) (for example, `WinMain = 0002:000003e0`)
  * **S\_LDATA32**,**S\_GDATA32** (DATASYM32) : local/global data (additonal properties: type index; 32-bit address) (for example, `char szMessageFormat[12] = "Hello, %s !"`)