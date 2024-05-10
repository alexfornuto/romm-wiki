Here is the recommended Folder/File structure in order to ensure your roms are properly identified.  Failure to follow one of these formats **may** work, but is not guaranteed nor supported.

*library* refers to RomM's internal container path, just replace it in your example with the directory containing your roms.

#### Recommended Format

This is the ideal structure to follow, every new installation should follow this unless absolutely necessary.

Inside of the library should be a single folder called ***roms***.  Herein you will place directories containing the platforms' folder name - see [here](https://github.com/rommapp/romm/wiki/Supported-Platforms) for the list of standard supported platforms, and refer [here](https://github.com/rommapp/romm/wiki/Custom-Platforms) for platforms that are not on the list.

##### Single File ROMs

```/library/roms/platform/rom_files```

![](assets\single-file-structure.svg)

##### Multi-File ROMs

```/library/roms/platform/rom_name/rom_files```

![](assets\multi-file-structure.svg)

#### Fallback Format

> To Be Implemented
