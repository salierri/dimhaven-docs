# Puzzle System (Basic implementation)

## The fundamentals

Allows the creation of relatively simple puzzles entirely by placing components

Puzzles consist of a PuzzleGroup script and any number of PuzzlePart types as children

PuzzleGroup will check weather all Parts underneath it are completed and will mark itself as completed

## PuzzleGroup

Either `ContinousCheck` has to be ON (For checking the completion every frame) or `CheckOnClick` collider needs to be set

## Dial

> Currently the only existing Puzzle Part

Needs an Animation component next to itself, and will set it up with Pause events based on `NumbersCount`

**The same Animation that it uses cannot be used on a Dial that has less/more numbers, as events will apply to all instances of the same Animation**

`Collider` will default to its own collider if not set
