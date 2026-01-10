### wl-gammarelay-brightnessctl-applet

#### Control wl-gammarelay-rs via applet

![wl-gammarelay-brightnessctl-applet demo](./doc/wga.gif)

`wl-gammarelay-brightnessctl-applet` is a small desktop applet for controlling `wl-gammarelay-rs` via DBus, and backlight brightness separately with `brightnessctl`

This applet is written in Rust and provides a [Slint](https://github.com/slint-ui/slint) UI.

The applet is configurable:

```
Control wl-gammarelay-rs via applet.

Usage: wl-gammarelay-brightnessctl-applet [OPTIONS]

Options:
  -i, --hide-invert
          Hides switch that controls Invert
  -t, --hide-temperature
          Hides slider that controls Temperature
  -b, --hide-brightness
          Hides slider that controls Brightness
  -g, --hide-gamma
          Hides slider that controls Gamma
  -c, --hide-caret
          Hides "speech bubble" caret at bottom of applet
  -l, --hide-labels
          Hides text labels of control widgets
  -v, --hide-value
          Hides text value of active control widgets
  -f, --never-fade
          Set this flag to never automatically fade the window
  -p, --outer-padding <OUTER_PADDING>
          Set applet window outer padding [default: 8]
  -x, --window-width <WINDOW_WIDTH>
          Set applet window width (horizontal) [default: 100]
  -y, --window-height <WINDOW_HEIGHT>
          Set applet window height (vertical) [default: 220]
  -T, --default-temperature <DEFAULT_TEMPERATURE>
          'Reset' value for temperature. (1000 - 10000) [default: 6500]
  -B, --default-brightness <DEFAULT_BRIGHTNESS>
          'Reset' value for brightness. (0.0 - 1.0) [default: 1]
  -G, --default-gamma <DEFAULT_GAMMA>
          'Reset' value for gamma. ( 0.5 - 1.5) [default: 1]
  -h, --help
          Print help
  -V, --version
          Print version
```

#### Usage

Click, drag, or scroll the sliders to change the values. `shift + scroll` changes the value in smaller increments. Right-clicking a slider will set it to the default value. If you resize windows in your compositor with the `meta` key, the applet window can be resized. Pressing `escape` will close the window.

#### Changes

In progress:

- [ ] Adding back tempreature, gamma, and invert color bars.
- [ ] Fixing weird behavior when using brightnessctl elsewhere while applet is running

0.2.1 changes:

- [x] Reworked brightness bar to use brightnessctl instead of wl-gammarelay
- [x] Removed temperature, gamma, and invert color bars

0.1.4 changes:

- [x] Text label showing set value.
- [x] Lock fade if shift or meta pressed.
- [x] Option to never fade out.
- [x] Esc to close window.
- [x] Configurable default value.
- [x] Right-click to reset to default.
- [x] Temperature click conversion (bugfix)

0.1.3 changes:

- Switch to blocking zbus proxy.
- Round temperature adjustments to nearest 100.
- Safely mutate settings, call DBus proxy from threads.

Known issue(s):

- Manually changing the brightness with brightnessctl while the applet is running breaks it. Restarting the applet fixes the behavior

Contribution welcome.

#### Getting Started

Install [wl-gammarelay-rs](https://github.com/MaxVerevkin/wl-gammarelay-rs) and [brightnessctl](https://github.com/Hummer12007/brightnessctl) as needed.

To build `wl-gammarelay-brightnessctl-applet`, here is a suggestion:

```bash
git clone https://github.com/OP-idk/wl-gammarelay-brightnessctl-applet.git
cd wl-gammarelay-brightnessctl-applet
cargo build --release
ln -s $(pwd)/target/release/wl-gammarelay-brightnessctl-applet ~/bin/wl-gammarelay-brightnessctl-applet
```

#### Configuration in Waybar

Suggestion:

```json
"custom/wl-gammarelay-applet": {
    "format": "❍ {}",
    "exec": "wl-gammarelay-rs watch {t}",
    "on-click": "wl-gammarelay-brightnessctl-applet",
    "on-scroll-up": "busctl --user -- call rs.wl-gammarelay / rs.wl.gammarelay UpdateTemperature n +100",
    "on-scroll-down": "busctl --user -- call rs.wl-gammarelay / rs.wl.gammarelay UpdateTemperature n -100"
},
```

Different configurations not widely tested. Feedback welcome.

#### Configuration in Sway

Add to `~/.config/sway/config`:

```swayconfig
for_window [app_id="wl-gammarelay-brightnessctl-applet"] sticky enable, move position cursor, move up 20
```

#### Configuration in Hyprland:
Add to `"~/.config/hypr/hyprland.conf"`

```
windowrule = float true, move cursor_x 50, match:class wl-gammarelay-brightnessctl-applet`
```

#### Curiosity

**Only brightness slider is available right now, same effect as using `-itg`. All other flags should still work.**

```bash
# Open a simple window that does not automatically close.
wl-gammarelay-brightnessctl-applet -fc

# Display only brightness and gamma with caret.
wl-gammarelay-brightnessctl-applet -it -p0 -x175

# Display only the unlabeled gamma slider.
wl-gammarelay-brightnessctl-applet -bltic

# Display a dark, empty square.
# Could this be the perfect GUI?
wl-gammarelay-brightnessctl-applet -bigtlc -p 100
```
