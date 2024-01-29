# Independent Behaviours

## Spline Knot Mover / Multi Spline Knot Mover

> Change a Spline Knot in real time with an additional transform.

Both scripts will use their own transforms, so they need to be attached to the animated GameObject.

SplineKnotMover will change the Knot global position to its own, while MultiSplineKnotMover will only use the movement delta (change in transform position), but can apply the same change to multiple knots in multiple splines.

The `KnotId` is numbered from 0.

## Omni-Directional Rigidbody

> Limits a Rigidbody's rotation on one axis to a single direction

If the Rigidbody gets a rotation that would rotate it in the forbidden direction, the rotation will be reversed at the end of the frame.

## Outside GameObject Toggle

> Animate GameObjects that are not children of the Animation component

When the component activates, turns on `ObjectsToTurnOn`, and off the other, so the component should be turned off at start for expected behaviour.

The `SetupDefaults` setting will enable/disable the contained GameObjects, _however_, it only works if the GameObject on which the OutsideGameObjectToggle component is attached, is ON at the start of the game.

## Sound Delay

**This is deprecated since FMOD, can be updated to work**

## NemBugosSplineAnimate

> Same as SplineAnimate, but throws no Exception when no spline is added, should be used on Seagulls

## LuxWater_SetToGeristnerHeight

> Moves **and rotates** GameObject according to waves in the water

## SlowZone

> Slows player while inside the trigger, has to be on the same GameObject as a trigger collider

## FogSzabalyzo

> Sets Fog color, Density and Skybox color

Component should only be enabled while in use and actively changing

Currently always sets all 3 parameters, can be updated to have toggles

## TransformLineRenderer

> Updates LineRenderer to match transform points

Component should only be enabled while in use and actively changing

Transform positions will always be global

Automatically adds itself as the first point, but does not require to actually use self position

## FastTravel

> Fades to black and teleports player to specified Transform

## ResetPlayerPosition

> Instantly teleports player to the transform it is attached to

## BigPanelHintSystem

> Enables interface hints in the world without the use of a CineCam

Has to be on a `drag_system` layer GameObject with a Collider

## Reading

> Enables the Reading GUI while clicked on this GameObject

Can be used with either InspectHandler or CineCamera

`Type` can be anything, but needs to be setup in /CANVAS/readings_guis with the exact name

## Animation Delay

The script has to be on the same GameObject as the Animation component.

The `Speed` variable is transferred to the animation, so it can be used to play the same animation at different speed on different GameObjects.

## Audio Manager

**This is deprecated since FMOD, can be updated to work**

An easy audio system, that can play a random sound if an animation starts, and can pair multiple audio clips to multiple animations, reducing the need to manually start them.

## FMOD Parameter Animator

> Change the `Parameter` of an FMOD StudioEventEmitter from animation.

Script can be placed anywhere, `Target` needs to be set, and `Value` changed from animation.

Currently only works with the **first** parameter of the target Emitter, this should be enough in 95% of the cases since we usually only have one FMOD parameter per Event, if it is somehow not enough, this can be updated to support multiple Parameters.

## Look At Camera

> Feature-rich Transform rotator

The `Axis` and `UpAxis` settings probably contain bugs, if they need to be used, contact Geri :D
