# Archipelago Notes

## Generate yaml file for each player

For each player, open the "Options Page" for their game of choice (e.g.
[Doom2](https://archipelago.gg/games/DOOM%20II/player-options), [Super Metroid](https://archipelago.gg/games/Super%20Metroid/player-options)),
**enter your player name**, configure the game, and hit "Export Options" to
download `<name>.yaml`. You should end up with a yaml file for each player.

## Upload all the yaml files to archipelago's server

"Generate" a game, starting at [this page](https://archipelago.gg/generate).
Optionally set a password and configure some things and hit "Upload Files", and
select all the yaml files generated in the previous step. Archipelago will
generate a game server with a host and port (e.g. `archipelago.gg:12345`).
Hit "Create Room" to see the server details. You'll need these for connecting
your game instance to the server so all the game instances can communicate with
each other.

## Run the Archipelago Text Client to monitor the game

Each player should do this separately.
Download the Archipelago application from
[here](https://github.com/ArchipelagoMW/Archipelago/releases). It's a collection
of helper tools for setting up randomizers. Press the "Text Client" button to
open a new window, and at the top of the window enter the game server address
(e.g. `archipelago.gg:12345`) and hit "Connect". On the "Archipelago" tab (ie.
not the "Hints" tab) you'll get a welcome message from the server, and it will
prompt "Enter slot name:". Type your player name (what you entered when
generating the game in the previous step, and your player's yaml file without
the .yaml) and hit enter. There are various commands for interacting with the
server from the text client, including `!hint` which tells you where items are
in the different connected games.

## Start your game

These instructions will obviously depend on the specific game.

### Doom2 (on NixOS)

E.g. if your player name is "steve" and the server is at `archipelago.gg:57421`,
run:

```
git clone https://github.com/gridbugs/apdoom
cd apdoom
git checkout nix
nix-build
cd result/pkg
./crispy-apdoom -apserver archipelago.gg:57421 -applayer steve -iwad /path/to/DOOM2.WAD -game doom2
```

### Super Metroid (on snes9x-emunwa on NixOS)

First, legally acquire a copy of Super Metroid, probably in a file named
something like "Super Metroid (Japan, USA) (En,Ja).sfc".

Then, from the Room page, download the patch file for your game. It will have
the file extension ".apsm".

Then from the Archipelago application, click "Open Patch" and select the .apsm
file. When prompted for a ROM, select the Super Metroid ROM. This will open a
page in your browser and download a .sfc file, which is the patched Super
Metroid ROM that we'll eventually open in an emulator.

Now we need to build an emulator that supports network access:

```
git clone https://github.com/gridbugs/snes9x-emunwa
cd snes9x-emunwa
git checkout nix
export NIXPKGS_ALLOW_UNFREE=1
nix-build
cd result/bin
./snes9x-gtk
```

Then in the UI, go Options->Preferences->UI, and tick "Enable Emulator Network
Access Support". Then load the patched ROM into the emulator. The Archipelago
application runs a server in the background that the SNES emulator connects to,
and it should automatically open a text client window with a log message like
"Attached to emunwa://127.0.0.1:48880".

## Cheating

It seems like it's possible to soft-lock all the games at once. To grant a
player an item to unblock everyone, the text client has a `/items` command
which lists all the items in a game. To get an item without picking it up
in-game, run `!getitem "Item Name"` (yes some commands start with `!` and other
commands start with `/`). If you don't use the right name you'll get a
suggestion for a similarly named item.
