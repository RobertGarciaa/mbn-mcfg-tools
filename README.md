# MBN MCFG Tools

This project provides a python library/application for parsing Qualcomm MBN configuration
files and the contained configuration items.

> [!WARNING]
> The format assumed by the parsers results from reverse engineering and is partially copied from
> similar projects (see [Related Repos](#related-repositories) below). We do NOT know how accurate
> the format is and how much the format changes between differing versions of configuration files.
> This uncertainty of whether we got the format right is especially true for the [parsers for
> individual configuration items](src/mbn_mcfg_tools/items_generated.py) which were auto generated
> from [here](https://github.com/JohnBel/EfsTools/tree/master/EfsTools/Items) and whose results are
> often inaccurate.

## MBN MCFG Files

MBN files are ELF files loadable by Qualcomm modems used not only to load executables but also to
load configuration. When used for modem configuration, these ELF files contain a Qualcomm-specific
segment — the MCFG segment likely standing for modem configuration — containing the configuration
and a secure boot header. However, the modems we tested with only checked the hashes in the secure
boot header and ignored wrong/missing signatures for MBN MCFG files.

The MBN modem configuration file format (or what we assume it to be) can be found [here](FORMAT.md).

## Usage

Our package provides a CLI tool to pack/unpack MBN files.

> [!INFO]
> When modifying an extracted MBN file, please note that currently, when changing a value of type
> "bytes", changes to its "ascii" property are ignored. Furthermore, only changes to the file `meta`
> and the files in the `files` directory are packed into an MBN file when using our tool to repack a
> file.

To extract the file `row_common.mbn` into the directory `row_common_extracted`:
```shell
mbn-extract -e row_common.mbn row_common_extracted
```

To pack the extracted configuration file `row_common_extracted` into the MBN file
`row_common_packed.mbn`:
```shell
mbn-extract -p row_common.mbn
```

To check the hashes in the secure boot header for validity:
```shell
mbn-extract -c row_common.mbn
```

## Related Repositories

* [EfsTools](https://github.com/JohnBel/EfsTools)
* [mcfg\_tools](https://github.com/Biktorgj/mcfg_tools)
* [qtestsign](https://github.com/msm8916-mainline/qtestsign)
* [msm8909w-law-2-0\_amss\_standard\_oem](https://github.com/ele7enxxh/msm8909w-law-2-0_amss_standard_oem)