# FMOD upgrade

### After upgrading the Unity plugin:

- Go to `StudioEventEmitter.cs` and add the following code _before_ `protected override void OnDestroy()`:

```
        // Custom Geri modifications
        protected override void OnDisable()
        {
            if (!RuntimeManager.IsSceneExiting || EventStopTrigger == EmitterGameEvent.ObjectDisable)
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

- Go to `EventHandler.cs` and modify the following line:

```
        private void OnDisable()
```

to

```
        protected virtual void OnDisable()
```

- Try it out!
