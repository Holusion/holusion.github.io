---
  layout: documentation
  title: Stingray
  abstract: Stingray controllable video playback engine compile options
---

# Compiling stingray

## Requirements

The `configure` script should list any missing requirement. Here is a non-exhaustive list of required dependencies :

- libSDL2
- ffmpeg libs. Precisely :
  - libavcodec
  - libavformat
  - libavutil
- libpthread (should be included in most systems)

## Compile

    ./autogen.sh
    ./configure
    make
    make install

Configure script is not distributed precompiled (hence the `autogen.sh`) until we have a stable release channel.

## Configure options

Stingray already provides a range of configure options. Much are feature-flags which may or may not be included in the default release. It helps keep the codebase tidy with isolated features, while encouraging contribution.

Here is a comprehensive list of major options, considered stable :

#### modules

*default : enabled. Disable with* `--disable-modules`

Disable libltdl and dynamic modules loading. Be aware that only keyboard and (if enabled) mouse control will be available in this mode.


#### mouse control

*default : disabled. Enable with* `--enable-mouse`

allow playback speed to be controlled by the mouse right/left movement.

#### autoexit

*default : disabled. Enable with* `--enable-autoexit`

Exit stingray after a few seconds of inactivity. Inactivity is calculated as "time with no change in playback speed".
