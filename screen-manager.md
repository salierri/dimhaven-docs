# ScreenManager

ScreenManager is the basic system for handling blocking functions (Esc menu, Inventory, etc.)

This is done by subscribing as a system at scene `Start`, and getting a callback whenever activated and deactivated.

## Subscription

Create a bool function with a simple bool parameter:

```c#
private bool setActive(bool value)
{
    // Example implementation
    isActive = value;
    gameObject.SetActive(value);
    return true;
}
```

The `value` bool parameter indicates wether it is a foreground or background request.

The `return` parameter can be used to cancel the request, if the toggle function returns `false`, the keypress will be ignored.

Then subscribe as a ScreenConsumer in `Start`:

```c#
ScreenManager.NewScreenConsumer("inventory", setActive);
```

It has an automatic function that watches the key press if it is specified in `InputManager` with the same name used here.

## `master: true`

The subscription has an additional option, master mode, that makes it so the consumer can only be turned off by its own key, and not other keys.

For example the esc menu should only be cancelled by pressing Escape, and not by opening the inventory.

```c#
ScreenManager.NewScreenConsumer("escmenu", setActive, master: true);
```

## Manual Foreground / Background request

Consumers can also be activated without the automatic keypress function, using:

```c#
ScreenManager.RequestForeground("inventory");
ScreenManager.RequestBackground("inventory");
```

A Foreground request will fail if another consumer is already in the foreground.

## Blocking

The automatic toggling can be blocked using:

```c#
ScreenManager.BlockUI();
ScreenManager.UnblockUI();
```

If anything is in the foreground when a blocking occurs, it will go to the background.
