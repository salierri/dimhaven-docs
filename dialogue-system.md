# Systems notes

## Input System

#### Checklist for a new input:

- Add to Project Settings / Input System Package
- Make sure to have exactly 2 Keyboard/Mouse bindings (empty if necessary), and tag those bindings
- If button should be rebindable:
  - Add button to `/CANVAS/esc_menu_container/esc_menu/settings/settings_panel/viewport/content/controls`
  - Add binding to `BindingManager`
  - Add label to `MenuTexts`
  - Add translation to yaml

#### Notes:

- The "Move" Action should not be interfered with, as it is referenced by Id from `BindingsManager`
- The "left" and "right" actions exist in code, but they are just the sum of "strafe_left" and "turn_left" actions
