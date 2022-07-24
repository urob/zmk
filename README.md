# ZMK Firmware: Personal fork

This is my personal ZMK fork containing various experimental features used in
my [zmk-config](https://github.com/urob/zmk-config/). This branch is up to date
with the latest ZMK upgrade to Zephyr 3.5. A legacy version
of this branch for Zephyr 3.2 can be found [here](https://github.com/urob/zmk/tree/main-3.2).

**Note**: I am in the process of moving to a modular ZMK setup and no longer use this branch myself.
Instead, I am building against [going-modular](https://github.com/urob/zmk/tree/going-modular),
which closely tracks upstream ZMK master (as of today there are only two additional commits on top
of master), adding all other features via ZMK modules.

---

Note that the branch now includes the official mouse PR #2027. If you are updating from an earlier
version with the original mouse PR #778, then the following
adjustments are necessary in your `zmk-config`:

- `&mwh` --> `&msc`
- `MOVE_VERT` --> `MOVE_Y` or `MOVE_LEFT`/`MOVE_RIGHT`
- `MOVE_HOR` --> `MOVE_X` or `MOVE_UP`/`MOVE_DOWN`
- `SCROLL_VERT` --> `MOVE_Y` or `SCRL_UP`/`SCRL_DOWN`
- `SCROLL_HOR` --> `MOVE_X` or `SCRL_LEFT`/`SCRL_RIGHT`

E.g., this is my current [mouse
configuration](https://github.com/urob/zmk-config/blob/main/config/mouse.dtsi)
using the new api.

---

Below is a list of features currently included in this branch _on top of_
the official ZMK master branch.

- **pointer movement/scrolling** - PR #2027
- **tri-state (aka swapper)** - PR [#1366](https://github.com/zmkfirmware/zmk/pull/1366)
- **smart-word** (PR [#1451](https://github.com/zmkfirmware/zmk/pull/1451))
- **on-release-for-tap-preferred** - [tweak](https://github.com/celejewski/zmk/commit/d7a8482712d87963e59b74238667346221199293) by Andrzej
- **zen-tweaks** - [display & battery improvements](https://github.com/caksoylar/zmk/tree/caksoylar/zen-v1%2Bv2) by Cem Aksoylar

In order to use this branch with Github Actions, replace the contents of `west.yml` in
your `zmk-config/config` directory with the following contents:

```
manifest:
  remotes:
    - name: urob
      url-base: https://github.com/urob
  projects:
    - name: zmk
      remote: urob
      revision: main
      import: app/west.yml
  self:
    path: config
```
