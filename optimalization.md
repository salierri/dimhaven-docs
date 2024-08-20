# Optimalization

## Unity functions

- `GameObject.Find()` costs around 4ms at the moment (2024 aug), so if used in the same hierarchy, consider using `transform.Find()`
- `FindObjectsOfType()` is _very_ expensive, avoid using it even in startup functions (around 30x more expensive than iterating through a much larger array and picking out the GameObjects ourselves)
