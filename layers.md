# Layers

- no_spotlight_cookie_plz - used on specific Spot Lights to ignore their cookies
- glass - blocks all raycasts and collides, but does not block flares
- non_colliding_only_unity_raycast - currently used for InspectHandler objects
- ignore_player - non-raycasted, player can pass through
- player - used in a lot of places, shouldn't be :D
- only_player_blocker - non-raycasted, player cannot pass through
- slot - raycasted, player can pass through
- drag_system - special layer for the dragging raycasts, ignores collision
- noPhysics, subPhysics - currently unused (was used for the rotating door, not needed anymore)
- RD_ layers - only affects render distance, raycasted and collided
- paintlayer - only rendered by LineRenderer_Camera, that camera only renders this layer
