# Trigger-Response System

## Two-part animation response (Paused door-handle animations)

##### Note: It only works with `InteractTrigger` components, as it uses the drag-system

### Setup

Attach a `TwoPartAnimationResponse` Component next to the Animation, so that Animation Events can be added.

The `TwoPartAnimationResponse` script does not need to be set up, it sets up automatically from the Animation and Default Clip, setup is only necessary if default clip is not used.

Add the Animation Events to the used animations, and use the following methods:

- `Pause()` - OnClick animation happens before the `Pause`, and drag animation happens after
- `ForwardPause()` - Same as `Pause()`, but only happens when the animation is playing forward
- `ReversePause()` - Same as `Pause()`, but only happens when the animation is playing backward

##### So for a classic open-close door setup, two events has to be added, a `ForwardPause` and a `ReversePause` after the corresponding handle presses.

Lastly, add the Animation GameObject to the `InteractTrigger` `Responses` array normally, and it should be good to go!
