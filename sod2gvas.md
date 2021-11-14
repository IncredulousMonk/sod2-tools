# State of Decay 2 save file notes

## GVAS file structure
A State of Decay 2 save file comprises a header, an LZ4-compressed block of properties, and a footer.

All multi-byte values are stored little endian.

### GVAS file header
This is what a header looks like at the moment. Some of these values may change over time.

| Offset (hex) | Length (bytes) | Details |
| --- | --- | --- |
| 0 | 4 | File signature (the characters "GVAS"). I guess it would be "SAVG" if read little endian. |
| 4 | 4 | Save game version (4) |
| 8 | 4 | Package version (506) |
| c | 2 | Engine major version (4) |
| e | 2 | Engine minor version (13) |
| 10 | 2 | Engine patch version (2) |
| 12 | 4 | Engine build version (0) |
| 16 | 8 | Engine build ID ("UE4") |
| 1e | 4 | Custom format version (3) |
| 22 | 4 | Custom format data count (27) |
| 26 | 540 | Custom format data values (a table of 27 GUID/integer pairs) |
| 242 | 4 | An unknown value (0x0006c996). Is this the SoD2 save file version? |
| 246 | 19 | Save game type ("DaytonSaveGame") |
| 259 | 4 | Another unknown value (3) |
| 25d | 4 | The length of the LZ4-compressed data block |
| 261 | 4 | The length of the data block after decompression |

The custom format data is a mystery to me.

## Properties block
This part of the file is where all of the interesting data is stored. Some of the "StructProperty" structure types are common to other GVAS files, and some are specific to State of Decay 2. This block is stored in an LZ4-compressed format, but I'll describe how the properties look when uncompressed.

This information is very much a work in progress, but these are the property types that I've encountered so far.

### Simple properties
| Name length | Name | Type length | Type | Data length | Padding? | Struct type length | Struct type | Padding? | Value |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *4 bytes* | "None" | *4 bytes* (0) |
| *4 bytes* | *Name* | *4 bytes* (12) | "StrProperty" | *8 bytes* | *1 null byte* | | | | *String* |
| *4 bytes* | *Name* | *4 bytes* (13) | "NameProperty" | *8 bytes* | *1 null byte* | | | | *String* |
| *4 bytes* | *Name* | *4 bytes* (12) | "IntProperty" | *8 bytes* (4) | *1 null byte* | | | | *Int32* |
| *4 bytes* | *Name* | *4 bytes* (14) | "FloatProperty" | *8 bytes* (4) | *1 null byte* | | | | *Float* |
| *4 bytes* | *Name* | *4 bytes* (15) | "DoubleProperty" | *8 bytes* (8) | *1 null byte* | | | | *Double* |
| *4 bytes* | *Name* | *4 bytes* (13) | "BoolProperty" | *8 bytes* (0) | | | | | *Int16* |
| *4 bytes* | *Name* | *4 bytes* (13) | "TextProperty" | *8 bytes* | *1 null byte* | | | | *Multiple strings* |

### Array of simple properties
| Name length | Name | Type length | Type | Data length | Element type length | Element type | Padding? | Element count | Value |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *4 bytes* | *Name* | *4 bytes* (14) | "ArrayProperty" | *8 bytes* | *4 bytes* | *Type name* | *1 null byte* | *4 bytes* | *Elements* |

### Structure properties
| Name length | Name | Type length | Type | Data length | Padding? | Struct type length | Struct type | Padding? | Value |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *4 bytes* | *Name* | *4 bytes* | "StructProperty" | *8 bytes* (16) | | *4 bytes* (5) | "Guid" | *17 null bytes* | *GUID* |
| *4 bytes* | *Name* | *4 bytes* | "StructProperty" | *8 bytes* (8) | | *4 bytes* (9) | "DateTime" | *17 null bytes* | *DateTime* |
| *4 bytes* | *Name* | *4 bytes* | "StructProperty" | *8 bytes* | | *4 bytes* (21) | "CharacterVarietySave" | *17 null bytes* | *?* |

#### A note about strings
The UE4 representation of strings seems to be a hybrid between Pascal-style (stored length) strings and C-style (null terminated) strings. All strings are stored as a 4-byte (little endian) length value, followed immediately by a null-terminated string. The length includes the null character.

## File footer
At the end of a save file, after the compressed block, there is a 40-byte chunk of data. This chunk seems to be all zeroes in SaveUser.sav, and random(?) bytes in the other save files. If this data contains checksums, or anything else that's meaningful, I would very much like to know about it.

---

This document © 2021 by Matthew Gill is licensed under [CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
