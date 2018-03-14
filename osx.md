# ALT+Space non breaking space

When entering alt+space on osx there’s a non-breaking space inserted which is sometimes problematic when working with markdown. This can easily be disabled system-wide that it will not insert a non-breaking space but a normal space.

Modify (or create if not exists) the file: `~/Library/KeyBindings/DefaultKeyBinding.dict`:

    {
        "~ " = ("insertText:", " ");
    }

Logout/Login or restart computer to let changes take effect.
