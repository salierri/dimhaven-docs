# Quick-start Guide for Lynxbyte

## Scenes

3 scenes are required for a build, `Dimhaven_main_scene`, `Dimhaven_menu_scene` and `Dimhaven_load_scene`.

## Dev tools

- F10 brings up the dev console, and typing `yes i am` enables all the dev features from a non-development build

- If the game is in dev mode (either Console is enabled from config, or `yes i am` has been typed) F1 brings up the - zone specific - dev panel

- Also in dev mode, pressing `Y` toggles free-flight

## Most useful console commands

- `unlimitedpower` or `ulp` - Toggles energy in the areas that require energy. In every area, the player has to setup power, but testing optimalization requires lights, this is a must-have command for profiling.

- `showblockers` - enables visual for blocker colliders, useful for debugging out-of-bounds

- `add {itemname}` - adds the given item, item names are under the `inventory_slot_item` prefab

- `solve {puzzle}` - solves given puzzle, `solve help` displays supported puzzles

- `devhints` - toggles in-game hints to some puzzles where a simple solution is enough

- `cycle {time-of-day}` - sets the current time of day to either `daytime`, `sunset` or `night`. Also useful for profiling, and also included in the F1 dev-panel as well.

## Config

Config can be setup in main scene on the `PROGRAM` GameObject.

Most useful config options:

- Console - if turned on, the game is running in "dev mode", console doesn't require password, and Y flight is enabled.

- No Start Anim - self-explanatory, basically mandatory in development mode

- Clean Start - removes saved games and resets PlayerPrefs on scene start

