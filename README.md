# zmk-keyboard-umbra

ZMK shield definition for the [Umbra](https://github.com/skarrmann/umbra)
24-key direct-wire keyboard.

The shield is a `zmk,kscan-gpio-direct` shield with a 1x24 matrix transform.
It binds GPIO pins directly (`&gpio0 N`) rather than going through an
interconnect nexus, so it is hardware-agnostic — pair it with any ZMK board
that exposes the relevant `&gpio0` pins.

The author's daily-driver setup pairs this shield with the Waveshare
RP2040-Plus via the
[zmk-component-waveshare-rp2040-plus](https://github.com/ryanfkeepers/zmk-component-waveshare-rp2040-plus)
module.

## Use in a zmk-config

In your `config/west.yml`, add this module as a remote and project:

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: ryanfkeepers
      url-base: https://github.com/ryanfkeepers
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: zmk-keyboard-umbra
      remote: ryanfkeepers
      revision: main
  self:
    path: config
```

Then in `build.yaml`:

```yaml
include:
  - board: <your_board>
    shield: umbra
```

The default keymap shipped here is opinionated — copy it to
`config/umbra.keymap` in your zmk-config and edit there to override.

## Layout

```
boards/shields/umbra/
  Kconfig.shield          # SHIELD_UMBRA flag
  Kconfig.defconfig       # ZMK_KEYBOARD_NAME
  umbra.overlay           # kscan + matrix transform + physical layout
  umbra.keymap            # default keymap (heavily personalized)
  umbra.conf              # shield-level Kconfig
  umbra.zmk.yml           # ZMK metadata
zephyr/module.yml         # registers the board_root with Zephyr
```

## License

MIT. See `LICENSE`.
