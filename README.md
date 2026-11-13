# nofrendo (NES) for ESP32 emulators

The NES core of esp-box-emu: nofrendo, from [espressif/esp32-nesemu](https://github.com/espressif/esp32-nesemu) (the nofrendo-esp32 port), with esp-box-emu's changes (shared-memory state, save states, video in the board's frame buffers).

## Layout

- `nofrendo/`: the emulator core, as esp-box-emu has carried it (see below for its origin and changes).
- `port/`: the glue to the host firmware: `port/include/nes.hpp` is the API the
  firmware calls (init / run a frame / save / load / deinit, the video buffer for
  screenshots), `port/` implements it.

The port talks to the board through the `BoxEmu` singleton of
[esp-box-emu](https://github.com/esp-cpp/esp-box-emu) (`box-emu.hpp`: frame
buffers, `push_frame()`, `palette()`, `play_audio()`, `gamepad_state()`, ...).
[tab5-emu](https://github.com/esp-cpp/tab5-emu) provides the same API
(`using BoxEmu = Tab5Emu`), so the same sources build for both.

## Use

Add this repository as a submodule inside an ESP-IDF component and register its
sources from that component's `CMakeLists.txt` (each firmware keeps its own
component wrapper: the requirements, compile flags and linker fragment for
its target). See esp-box-emu's `components/nes/CMakeLists.txt`.

The history up to the split is esp-box-emu's history of `components/nes`.

## License

nofrendo is under the GNU Library General Public License, version 2 (see the headers of the files in `nofrendo/`). The port is under the same license.
