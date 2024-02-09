# Trigger-Response System

## The fundamentals

Triggers and Responses as MonoBehaviours, triggers can trigger and anti-trigger based on a player action (Click or walk into a collider)

Responses does not need to be setup in most cases, as Triggers only need GameObjects as responses, and if they find an object without a Response, they will try to populate it with the appropriate one.

Population rules:
- If the GameObject has an Animation component, but no AnimationResponse, then it gets one, with the default clip.
- If the GameObject has a CineCamera, but no CineCamResponse, it gets one.
- If the GameObject has **exactly one** StudioEventEmitter and no FmodPulseResponse, it gets one.
- If after all these, the GameObject still has no Response attached, it gets a ToggleResponse with the default state being the GameObject's current state.

### Trigger Loading Rules

- A Trigger that is On will have its Animation scrolled to its end.
- A Trigger that is Off, PingPong, and has a `ReverseAnimation` will have the `ReverseAnimation` scrolled to its end.
- A Trigger that is Off, PingPong, and does not have a `ReverseAnimation` will have the default animation scrolled to its beggining.
- A Trigger that is Off, and _not_ PingPong will be left as is.

**Important: Two triggers with the exact same name and hierarchy path should not exist, as save-load uses the path to differentiate between triggers**

## InteractTrigger

> The main entry point, "Trigger on click"

Non-intuitive bits:
- `Hold` variable only affects the indicator, drag needs to be setup differently
- `Blocking` only works together with Hold, and disables all other InteractTriggers while animating
- `NoItemAnimation` is only a convenience feature, it adds an AnimationResponse and fills it with the Animation's default clip, same as setting up a NoItemResponse yourself
- `DragTargetCollider` - if it is present, this Trigger will become a dragging Trigger. `ReverseDragCollider` is optional, if not present, the forward one will be used both ways
- `OrderedSave` should be On if multiple Triggers effect the same GameObject (ex. multiple Clips in the same Animation)
- `ConnectedTriggers` flips the other triggers when this one is fired, it should be used ex. on doors with two handles
- `BlockingAnimations` - this Trigger becomes uninteractable while any animation is playing in the list

## WalkIntoTrigger

Needs a 'trigger' Collider on the same GameObject

## AnimationResponse

- Automatically sets up the Animation if on the same GameObject
- If `OffLoop` is present, it will start playing on scene start
- `FadeTime` will effect all animation fades, loops included

## Two-part animation response

> Paused door-handle animations with a distinct click and drag part

**Note: It only works with `InteractTrigger` components, as it uses the drag-system**

### Setup

Attach a `TwoPartAnimationResponse` Component next to the Animation, so that Animation Events can be added.

The `TwoPartAnimationResponse` script does not need to be set up, it sets up automatically from the Animation and Default Clip, setup is only necessary if default clip is not used.

Add the Animation Events to the used animations, and use the following methods:

- `Pause()` - OnClick animation happens before the `Pause`, and drag animation happens after
- `ForwardPause()` - Same as `Pause()`, but only happens when the animation is playing forward
- `ReversePause()` - Same as `Pause()`, but only happens when the animation is playing backward

**So for a classic open-close door setup, two events has to be added, a `ForwardPause` and a `ReversePause` after the corresponding handle presses.**

Lastly, add the Animation GameObject to the `InteractTrigger` `Responses` array normally, and it should be good to go!

## CineCamResponse

> A special response, that exits all other Responses on the same Trigger when the CineCam exits

## FmodPulseResponse

> Turns on the GameObject for one frame only

## RandomPositionResponse

If only used for Position/Rotation, the other one still needs to be setup to its own Position/Rotation, or else it will default to 0-0-0. (If this component is used more often, I will update it to toggles)

## ToggleResponse

> Turns the GameObject on/off

## AutosaveResponse

> Triggers an Autosave or a Chapter save

Autosaves will always save on Slot0, and will overwrite the last autosave, while chapter saves will create a new slot and can only be overwritten manually

The `ChapterTextKey` should be a key from the locales yaml, located under `chapter_autosaves`
