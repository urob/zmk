# ZMK Firmware: Personal fork

This is my personal ZMK fork containing various experimental features used in
my [zmk-config](https://github.com/urob/zmk-config/). 

**Important**: I have moved to a modular ZMK setup and no longer use this branch myself. I will keep
a copy of this branch available as **[old-main](https://github.com/urob/zmk/tree/main-3.5)** but I
will no longer maintain it. 

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
