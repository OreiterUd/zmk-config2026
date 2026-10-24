# ZMK keycode reference

This guide explains how to write key references in this project's
`config/corne.keymap`. ZMK calls these **keycodes**. The complete list is large,
so this guide shows the usual names and how to find the full catalog.

## The basic form

Use `&kp` (key press) followed by the keycode name:

```dts
&kp A
&kp SPACE
&kp LEFT
```

For example, a keymap binding of `&kp A` sends the A key when that physical
key is pressed. Keycode names are written in uppercase. They are names, not
the characters to type: use `&kp SEMI` for the semicolon key, for example.

## Common keys

| Key or group | Keycode names |
| --- | --- |
| Letters | `A` through `Z` |
| Number-row digits | `N0` through `N9` |
| Enter, Escape, Space, Tab | `RET` (or `ENTER`), `ESC`, `SPACE`, `TAB` |
| Backspace and forward Delete | `BSPC`, `DELETE` (or `DEL`) |
| Arrows | `LEFT`, `RIGHT`, `UP`, `DOWN` |
| Home, End, Page Up, Page Down | `HOME`, `END`, `PG_UP`, `PG_DN` |
| Function keys | `F1` through `F24` |
| Punctuation | `MINUS`, `EQUAL`, `LBKT`, `RBKT`, `BSLH`, `SEMI`, `SQT`, `GRAVE`, `COMMA`, `DOT`, `FSLH` |
| Symbols that need Shift | `EXCL`, `AT`, `HASH`, `DLLR`, `PRCNT`, `CARET`, `AMPS`, `ASTRK`, `LPAR`, `RPAR`, `UNDER`, `PLUS`, `LBRC`, `RBRC`, `PIPE`, `TILDE` |

Some keys have both a long name and a short alias. For example, `RETURN` and
`RET` name the same key, as do `BACKSPACE` and `BSPC`. The complete catalog
shows the available spellings; prefer the current names listed there over any
name marked deprecated.

## Shifted symbols and keyboard layouts

The symbol names above send the symbol directly. For example, `&kp EXCL` sends
Shift+1 on a US layout. A plain punctuation key such as `&kp SEMI` sends the
key at that physical keyboard position; what character appears depends on the
keyboard layout selected by the computer. Other language layouts may produce
different characters.

You can also combine a modifier and a key yourself; see the next section.

## Modifier keys and shortcuts

Modifier **keys** can be sent just like other keys:

| Modifier | Left | Right |
| --- | --- | --- |
| Control | `LCTRL` | `RCTRL` |
| Shift | `LSHFT` | `RSHFT` |
| Alt | `LALT` | `RALT` |
| GUI (Windows/Command key) | `LGUI` | `RGUI` |

For a shortcut, wrap the keycode in a modifier function:

```dts
&kp LC(C)       // Left Control + C
&kp LG(SPACE)   // Left GUI + Space
&kp LS(A)       // Shift + A
&kp LC(LS(ESC)) // Control + Shift + Escape
```

Functions are `LC` (left Control), `LS` (left Shift), `LA` (left Alt),
`LG` (left GUI), and `RC`, `RS`, `RA`, `RG` for their right-side equivalents.
They can be combined or nested.

## Keypad, media, and other special keys

Not every keycode is a letter or key printed on a standard keyboard. The full
catalog also includes:

- **Keypad:** use names beginning with `KP_`, such as `KP_N1`, `KP_PLUS`,
  `KP_ENTER`, and `KP_DOT`.
- **Media controls:** names beginning with `C_`, such as `C_PLAY_PAUSE`,
  `C_VOL_UP`, `C_VOL_DN`, `C_MUTE`, `C_NEXT`, and `C_PREV`.
- **Applications and browser controls:** commonly names beginning with
  `C_AL_`, such as `C_AL_CALCULATOR` and `C_AL_WWW`.
- **International and language keys:** names such as `INT1`, `INT2`, `LANG1`,
  and `LANG2`.
- **Power and system controls:** names such as `SYSTEM_SLEEP`, `SYSTEM_WAKE_UP`,
  and `SYSTEM_POWER`.

Some computers or applications do not respond to every media, application, or
system key. Those keys are distinct from ordinary keyboard keys.

## Find the complete list

The authoritative, searchable catalog is ZMK's
[List of Keycodes](https://zmk.dev/docs/keymaps/list-of-keycodes). It groups
the supported names into keyboard, modifier, keypad, editing, media,
application, input-assist, and power/lock categories, with descriptions.

The keycode definitions used by this project are in ZMK's
[`keys.h`](https://github.com/zmkfirmware/zmk/blob/main/app/include/dt-bindings/zmk/keys.h)
header. The keymap makes them available with this include (already present in
`config/corne.keymap`):

```dts
#include <dt-bindings/zmk/keys.h>
```

## Keycodes versus ZMK behaviors

Keycodes describe keys or HID actions sent to the computer. ZMK behaviors
describe what a physical key does in the firmware. For example:

```dts
&kp A       // Send the A key
&mo 1       // Momentarily activate layer 1; this is a behavior, not a keycode
&bt BT_CLR  // Clear Bluetooth pairing; this is also a behavior
&trans      // Let the active lower layer fall through to the layer below
```

When editing this project's layers, use `&kp KEYCODE` for ordinary keys and
look up the appropriate behavior when you want a layer action, Bluetooth
action, macro, or other special firmware function.
