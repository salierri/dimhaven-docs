# Puzzle System (Using VIDE)

## The fundamentals

Using VIDE is pretty self-explanatory, we will only touch the customized bits.

#### FMOD event (and yaml key) naming

One FMOD event per line

The event name for a given line is the first 25 characters, following these rules:
- If after 25 characters we are in the middle of a word, we extend the name to the end of that word
- If the 25th character is a space, we remove that and the name will be 24 characters long
- If there are multiple lines with the same first 25 characters, the event name has to be specified using Extra Variables (__currently unimplemented, notify me if it happens :D__)

#### NPC tagging

The NPC nodes have to be tagged to specify the animable character. If a node belongs to the same character as the last already NPC visited node, it does not need to be tagged, and will instead use the last tag. This means that in most cases, only the first NPC node needs to be tagged.

Tags are the NPC's short name starting with a capital letter: `Lars`, `Zack`

## Conditions and Effects

Conditions and effects have to be setup in the Extra Variables part of the VIDE dialogue node:

![vide extra example](/vide_extra_example.png)

As there are multiple player lines per VIDE node, they have to be suffixed by the answer ID, ex: `remove_item_2` belongs to answer id 2

#### Player line conditions

If a given condition is not satisfied, the answer will not show up for the player.

Possible conditions:
- `remove_item_`: Answer will only show up if the player has the given item _anywhere_ in their inventory
- `prereq_`: The base condition, the Value will specify the exact check
  - `/PUZZLES/KISHVAR_GATE/...`: If the condition starts with a `/`, it will check whether the given GameObject is active. This can be useful for Dani to setup dialogue conditions without requiring additional code.
  - `photo_`: Whether the given photo challenge has already been completed, ex. `photo_first_picture`
  - `talked_`: Has the given dialogue node been visited (in the current dialogue) ex: `talked_4`
  - ...: Other options can also be used, but they are required to be implemented first.

#### Player line effects

These also need to be suffixed by the answer ID, and will execute when the player clicks the given answer.

Possible effects:
- `remove_item`: The player gives the specified item to the NPC.
- `track`: This one is a special, as it modifies the dialogue ordering behaviour. If a track is specified, after selecting the answer, the next nodes will be the ones from the track in the given order, and the VIDE arrows will be ignored. This is useful when the player line modifies the order of the NPC lines, but they are reused. Ex. after `track_0` `1 2 3 6` the next nodes will be 1, 2, 3, 6 in this order. The track nodes need to be separated by a space.

#### NPC line effects

As NPCs have only one line per node, they don't need to be suffixed.

Possible effects:
- `add_item`: The NPC gives the item to the player. If the player already has the specified item (ex. the node is visited again), nothing happens.
- `action`: Programmable action, will need to contact Geri before using these.
