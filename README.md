# State of Decay 2 saved game tools

A set of tools for working with save files from the game State of Decay 2.

Included so far are:
- **decomp** - a script to decompress a save file.
- **recomp** - a script to recompress a decompressed save file.

## Prerequisites
1. If you are not comfortable with a hex editor then these aren't the tools for you. Also, they are command-line scripts, not point and click.
2. You have to be willing to install the Erlang programming language.  Details may be found below.

## Installing Erlang
There is a Windows installer on the [Erlang downloads page](https://www.erlang.org/downloads). I'm using Erlang/OTP 24, but it's likely that the scripts will work with other versions.

The installer doesn't add the Erlang bin directory to your path, so please do that yourself; otherwise, the scripts won't run.

## Running the scripts

Before you run anything, first **backup your save files**! The scripts won't ask you if you want to overwrite an existing file.

### On Windows
The scripts must be run using the `escript` executable that comes with the Erlang installation. This will work only if the Erlang bin directory is in your path. Run them like this:

    escript decomp <compressed-filename> <decompressed-filename>

to decompress a save file. For example,

    escript decomp SaveGame0.sav save0

or:

    escript recomp <uncompressed-filename> <compressed-filename>

to recompress a save file that you previously decompressed. For example,

    escript recomp save0 SaveGame0.sav

### On Linux
The scripts start with a shebang for escript, so you just need to do a `chmod u+x` to make them executable. After that you can run them as:

    decomp <compressed-filename> <decompressed-filename>

to decompress a save file. For example,

    decomp SaveGame0.sav save0

or:

    recomp <uncompressed-filename> <compressed-filename>

to recompress a save file that you previously decompressed. For example,

    recomp save0 SaveGame0.sav

## Why Erlang?

I wrote this code in Erlang for two main reasons:
1. Erlang is a really good language for manipulating data at the bits-and-bytes level.
2. My Erlang skills were getting rusty, and I wanted to stretch them a bit.

If you are interested in learning to program in Erlang there is documentation at the [Erlang site](https://www.erlang.org/docs), but I would recommend starting with the excellent [Learn you some Erlang](https://learnyousomeerlang.com/content).

If any Erlang experts out there are horrified by my code, and my violation of all sane Erlang idioms, I would welcome critiques!

## Licence
The code is licensed under GPL-2.0-or-later. Please see the COPYING file for details.
