# FMOD upgrade

### After upgrading the Unity plugin:

- Go to `EventHandler.cs` and modify the following line:

```
        private void OnDisable()
```

to

```
        protected virtual void OnDisable()
```

- Go to `StudioEventEmitter.cs` and add the following code _after_ `public bool IsActive { get; private set; }`:

```
        // Custom Geri modifications
        public static bool IsSceneExiting;
```

- In the same file, add the following code _before_ `protected override void OnDestroy()`:

```
        // Custom Geri modifications
        protected override void OnDisable()
        {
            if (!IsSceneExiting || EventStopTrigger == EmitterGameEvent.ObjectDisable)
                base.OnDisable();
        }
```

- In the same file, modify the following line:

```
                    if (eventDescription.isValid() && isOneshot)
```

to

```
                    if (eventDescription.isValid())
```

- Try it out! (If it runs without errors, it's probably OK, one thing you can test is standing next to the middle landing arrow and loading a save file, there shouldn't be an arrow turn-off audio.)
