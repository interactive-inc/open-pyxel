---
applyTo: "**"
---

# Pyxel

https://github.com/kitao/pyxel

Pyxel is a retro game engine for Python.

With simple specifications inspired by retro gaming consoles, such as displaying only 16 colors and supporting 4 sound channels, you can easily enjoy making pixel-art-style games.

## How to Use

### Create Application

In your Python script, import the Pyxel module, specify the window size with the `init` function, and then start the Pyxel application with the `run` function.

```python
import pyxel

pyxel.init(160, 120)

def update():
    if pyxel.btnp(pyxel.KEY_Q):
        pyxel.quit()

def draw():
    pyxel.cls(0)
    pyxel.rect(10, 10, 20, 20, 11)

pyxel.run(update, draw)
```

The arguments of the `run` function are the `update` function, which processes frame updates, and the `draw` function, which handles screen drawing.

In an actual application, it is recommended to wrap Pyxel code in a class, as shown below:

```python
import pyxel

class App:
    def __init__(self):
        pyxel.init(160, 120)
        self.x = 0
        pyxel.run(self.update, self.draw)

    def update(self):
        self.x = (self.x + 1) % pyxel.width

    def draw(self):
        pyxel.cls(0)
        pyxel.rect(self.x, 0, 8, 8, 9)

App()
```

For creating simple graphics without animation, you can use the `show` function to simplify your code.

```python
import pyxel

pyxel.init(120, 120)
pyxel.cls(1)
pyxel.circb(60, 60, 40, 7)
pyxel.show()
```

### Run Application

A created script can be executed using the `python` command:

```sh
python PYTHON_SCRIPT_FILE
```

It can also be run with the `pyxel run` command:

```sh
pyxel run PYTHON_SCRIPT_FILE
```

Additionally, the `pyxel watch` command monitors changes in a specified directory and automatically re-runs the program when changes are detected:

```sh
pyxel watch WATCH_DIR PYTHON_SCRIPT_FILE
```

Directory monitoring can be stopped by pressing `Ctrl(Command)+C`.

### Special Key Controls

The following special key actions are available while a Pyxel application is running:

- `Esc`<br>
  Quit the application
- `Alt(Option)+R` or `A+B+X+Y+BACK` on gamepad<br>
  Reset the application
- `Alt(Option)+1`<br>
  Save the screenshot to the desktop
- `Alt(Option)+2`<br>
  Reset the recording start time of the screen capture video
- `Alt(Option)+3`<br>
  Save a screen capture video to the desktop (up to 10 seconds)
- `Alt(Option)+8` or `A+B+X+Y+DL` on gamepad<br>
  Toggles screen scaling between maximum and integer
- `Alt(Option)+9` or `A+B+X+Y+DR` on gamepad<br>
  Switch between screen modes (Crisp/Smooth/Retro)
- `Alt(Option)+0` or `A+B+X+Y+DU` on gamepad<br>
  Toggle the performance monitor (fps/`update` time/`draw` time)
- `Alt(Option)+Enter` or `A+B+X+Y+DD` on gamepad<br>
  Toggle fullscreen
- `Shift+Alt(Option)+1/2/3`<br>
  Save image bank 0, 1, or 2 to the desktop
- `Shift+Alt(Option)+0`<br>
  Save the current color palette to the desktop

## API Reference

### System

- `width`, `height`<br>
  The width and height of the screen

- `frame_count`<br>
  The number of the elapsed frames

- `init(width, height, [title], [fps], [quit_key], [display_scale], [capture_scale], [capture_sec])`<br>
  Initialize the Pyxel application with the screen size (`width`, `height`). The following options can be specified: the window title with `title`, the frame rate with `fps`, the key to quit the application with `quit_key`, the display scale with `display_scale`, the screen capture scale with `capture_scale`, and the maximum recording time of the screen capture video with `capture_sec`.<br>
  Example: `pyxel.init(160, 120, title="My Pyxel App", fps=60, quit_key=pyxel.KEY_NONE, capture_scale=3, capture_sec=0)`

- `run(update, draw)`<br>
  Start the Pyxel application and call the `update` function for frame update and the `draw` function for drawing.

- `show()`<br>
  Show the screen and wait until the `Esc` key is pressed.

- `flip()`<br>
  Refresh the screen by one frame. The application exits when the `Esc` key is pressed. This function is not available in the web version.

- `quit()`<br>
  Quit the Pyxel application.

- `reset()`<br>
  Reset the Pyxel application. Environment variables are preserved after reset.

### Resource

- `load(filename, [exclude_images], [exclude_tilemaps], [exclude_sounds], [exclude_musics])`<br>
  Load the resource file (.pyxres). If an option is set to `True`, the corresponding resource will be excluded from loading. If a palette file (.pyxpal) with the same name exists in the same location as the resource file, the palette display colors will also be updated. The palette file contains hexadecimal entries for the display colors (e.g. `1100ff`), separated by newlines. The palette file can also be used to change the colors displayed in Pyxel Editor.

- `user_data_dir(vendor_name, app_name)`<br>
  Returns the user data directory created based on `vendor_name` and `app_name`. If the directory does not exist, it will be created automatically. It is used to store high scores, game progress, and similar data.<br>
  Example: `print(pyxel.user_data_dir("Takashi Kitao", "Pyxel Shooter"))`

### Input

- `mouse_x`, `mouse_y`<br>
  The current position of the mouse cursor

- `mouse_wheel`<br>
  The current value of the mouse wheel

- `btn(key)`<br>
  Return `True` if the `key` is pressed, otherwise return `False`. ([Key definition list](python/pyxel/__init__.pyi))

- `btnp(key, [hold], [repeat])`<br>
  Return `True` if the `key` is pressed in that frame, otherwise return `False`. If `hold` and `repeat` are specified, after the `key` has been held down for `hold` frames or more, `True` is returned every `repeat` frames.

- `btnr(key)`<br>
  Return `True` if the `key` is released in that frame, otherwise return `False`.

- `mouse(visible)`<br>
  Show the mouse cursor if `visible` is `True`, and hide it if `visible` is `False`. The cursor's position continues to update even when it is hidden.

### Graphics

- `colors`<br>
  List of the palette display colors. The display color is specified by a 24-bit numerical value. Use `colors.from_list` and `colors.to_list` to directly assign and retrieve Python lists.<br>
  Example: `old_colors = pyxel.colors.to_list(); pyxel.colors.from_list([0x111111, 0x222222, 0x333333]); pyxel.colors[15] = 0x112233`

- `images`<br>
  List of the image banks (instances of the Image class) (0-2)<br>
  Example: `pyxel.images[0].load(0, 0, "title.png")`

- `tilemaps`<br>
  List of the tilemaps (instances of the Tilemap class) (0-7)

- `clip(x, y, w, h)`<br>
  Set the drawing area of the screen from (`x`, `y`) with a width of `w` and a height of `h`. Call `clip()` to reset the drawing area to full screen.

- `camera(x, y)`<br>
  Change the upper-left corner coordinates of the screen to (`x`, `y`). Call `camera()` to reset the upper-left corner coordinates to (`0`, `0`).

- `pal(col1, col2)`<br>
  Replace color `col1` with `col2` when drawing. Call `pal()` to reset to the initial palette.

- `dither(alpha)`<br>
  Apply dithering (pseudo-transparency) when drawing. Set `alpha` in the range `0.0`-`1.0`, where `0.0` is transparent and `1.0` is opaque.

- `cls(col)`<br>
  Clear screen with color `col`.

- `pget(x, y)`<br>
  Get the color of the pixel at (`x`, `y`).

- `pset(x, y, col)`<br>
  Draw a pixel of color `col` at (`x`, `y`).

- `line(x1, y1, x2, y2, col)`<br>
  Draw a line of color `col` from (`x1`, `y1`) to (`x2`, `y2`).

- `rect(x, y, w, h, col)`<br>
  Draw a rectangle of width `w`, height `h` and color `col` from (`x`, `y`).

- `rectb(x, y, w, h, col)`<br>
  Draw the outline of a rectangle of width `w`, height `h` and color `col` from (`x`, `y`).

- `circ(x, y, r, col)`<br>
  Draw a circle of radius `r` and color `col` at (`x`, `y`).

- `circb(x, y, r, col)`<br>
  Draw the outline of a circle of radius `r` and color `col` at (`x`, `y`).

- `elli(x, y, w, h, col)`<br>
  Draw an ellipse of width `w`, height `h` and color `col` from (`x`, `y`).

- `ellib(x, y, w, h, col)`<br>
  Draw the outline of an ellipse of width `w`, height `h` and color `col` from (`x`, `y`).

- `tri(x1, y1, x2, y2, x3, y3, col)`<br>
  Draw a triangle with vertices (`x1`, `y1`), (`x2`, `y2`), (`x3`, `y3`) and color `col`.

- `trib(x1, y1, x2, y2, x3, y3, col)`<br>
  Draw the outline of a triangle with vertices (`x1`, `y1`), (`x2`, `y2`), (`x3`, `y3`) and color `col`.

- `fill(x, y, col)`<br>
  Fill the area connected with the same color as (`x`, `y`) with color `col`.

- `blt(x, y, img, u, v, w, h, [colkey], [rotate], [scale])`<br>
  Copy the region of size (`w`, `h`) from (`u`, `v`) of the image bank `img`(0-2) to (`x`, `y`). If a negative value is assigned to `w` and/or `h`, the region will be flipped horizontally and/or vertically. If `colkey` is specified, it will be treated as a transparent color. If `rotate`(in degrees), `scale`(1.0 = 100%), or both are specified, the corresponding transformations will be applied.

- `bltm(x, y, tm, u, v, w, h, [colkey], [rotate], [scale])`<br>
  Copy the region of size (`w`, `h`) from (`u`, `v`) of the tilemap `tm`(0-7) to (`x`, `y`). If a negative value is assigned to `w` and/or `h`, the region will be flipped horizontally and/or vertically. If `colkey` is specified, it will be treated as a transparent color. If `rotate`(in degrees), `scale`(1.0 = 100%), or both are specified, the corresponding transformations will be applied. The size of a tile is 8x8 pixels and is stored in a tilemap as a tuple of `(image_tx, image_ty)`.

- `text(x, y, s, col)`<br>
  Draw a string `s` in color `col` at (`x`, `y`).

### Audio

- `sounds`<br>
  List of the sounds (instances of the Sound class) (0-63)<br>
  Example: `pyxel.sounds[0].speed = 60`

- `musics`<br>
  List of the musics (instances of the Music class) (0-7)

- `play(ch, snd, [sec], [loop], [resume])`<br>
  Play the sound `snd`(0-63) on channel `ch`(0-3). `snd` can be a sound number, a list of sound numbers, or an MML string. The playback start position can be specified in seconds with `sec`. If `loop` is set to `True`, the sound will loop. To resume the previous sound after playback ends, set `resume` to `True`.

- `playm(msc, [sec], [loop])`<br>
  Play the music `msc`(0-7). The playback start position can be specified in seconds with `sec`. If `loop` is set to `True`, the music will loop.

- `stop([ch])`<br>
  Stop playback of the specified channel `ch`(0-3). Call `stop()` to stop all channels.

- `play_pos(ch)`<br>
  Get the sound playback position of channel `ch`(0-3) as a tuple of `(sound_no, sec)`. Return `None` when playback has stopped.

### Math

- `ceil(x)`<br>
  Return the smallest integer that is greater than or equal to `x`.

- `floor(x)`<br>
  Return the largest integer that is less than or equal to `x`.

- `sgn(x)`<br>
  Return `1` when `x` is positive, `0` when it is `0`, and `-1` when it is negative.

- `sqrt(x)`<br>
  Return the square root of `x`.

- `sin(deg)`<br>
  Return the sine of `deg` degrees.

- `cos(deg)`<br>
  Return the cosine of `deg` degrees.

- `atan2(y, x)`<br>
  Return the arctangent of `y`/`x` in degrees.

- `rseed(seed)`<br>
  Sets the seed of the random number generator.

- `rndi(a, b)`<br>
  Return a random integer greater than or equal to `a` and less than or equal to `b`.

- `rndf(a, b)`<br>
  Return a random floating-point number greater than or equal to `a` and less than or equal to `b`.

- `nseed(seed)`<br>
  Set the seed of Perlin noise.

- `noise(x, [y], [z])`<br>
  Return the Perlin noise value for the specified coordinates.

### Image Class

- `width`, `height`<br>
  The width and height of the image

- `set(x, y, data)`<br>
  Set the image at (`x`, `y`) using a list of strings.<br>
  Example: `pyxel.images[0].set(10, 10, ["0123", "4567", "89ab", "cdef"])`

- `load(x, y, filename)`<br>
  Load an image file (PNG/GIF/JPEG) at (`x`, `y`).

- `pget(x, y)`<br>
  Get the color of the pixel at (`x`, `y`).

- `pset(x, y, col)`<br>
  Draw a pixel with the color `col` at (`x`, `y`).

### Tilemap Class

- `width`, `height`<br>
  The width and height of the tilemap

- `imgsrc`<br>
  The image bank (0-2) referenced by the tilemap

- `set(x, y, data)`<br>
  Set the tilemap at (`x`, `y`) using a list of strings.<br>
  Example: `pyxel.tilemap(0).set(0, 0, ["0000 0100 a0b0", "0001 0101 a1b1"])`

- `load(x, y, filename, layer)`<br>
  Load the `layer` (0-) from the TMX file (Tiled Map File) at (`x`, `y`).

- `pget(x, y)`<br>
  Get the tile at (`x`, `y`). A tile is represented as a tuple of `(image_tx, image_ty)`.

- `pset(x, y, tile)`<br>
  Draw a `tile` at (`x`, `y`). A tile is represented as a tuple of `(image_tx, image_ty)`.

### Sound Class

- `notes`<br>
  List of notes (0-127). The higher the number, the higher the pitch. Note `33` corresponds to 'A2'(440Hz). Rest notes are represented by `-1`.

- `tones`<br>
  List of tones (0:Triangle / 1:Square / 2:Pulse / 3:Noise)

- `volumes`<br>
  List of volumes (0-7)

- `effects`<br>
  List of effects (0:None / 1:Slide / 2:Vibrato / 3:FadeOut / 4:Half-FadeOut / 5:Quarter-FadeOut)

- `speed`<br>
  Playback speed. `1` is the fastest, and the larger the number, the slower the playback speed. At `120`, the length of one note becomes 1 second.

- `set(notes, tones, volumes, effects, speed)`<br>
  Set notes, tones, volumes, and effects using a string. If the length of tones, volumes, or effects are shorter than the notes, they will be repeated from the beginning.

- `set_notes(notes)`<br>
  Set the notes using a string made of `CDEFGAB`+`#-`+`01234` or `R`. It is case-insensitive, and whitespace is ignored.<br>
  Example: `pyxel.sounds[0].set_notes("g2b-2d3r rf3f3f3")`

- `set_tones(tones)`<br>
  Set the tones with a string made of `TSPN`. Case-insensitive and whitespace is ignored.<br>
  Example: `pyxel.sounds[0].set_tones("ttss pppn")`

- `set_volumes(volumes)`<br>
  Set the volumes with a string made of `01234567`. Case-insensitive and whitespace is ignored.<br>
  Example: `pyxel.sounds[0].set_volumes("7777 7531")`

- `set_effects(effects)`<br>
  Set the effects with a string made of `NSVFHQ`. Case-insensitive and whitespace is ignored.<br>
  Example: `pyxel.sounds[0].set_effects("nfnf nvvs")`

- `mml(code)`<br>
  Passing a [MML (Music Macro Language)](https://en.wikipedia.org/wiki/Music_Macro_Language) string switches to MML mode and plays the sound according to its content. In this mode, normal parameters such as `notes` and `speed` are ignored. To exit MML mode, call `mml()`. For more details about MML, see [this page](docs/faq-en.md).<br>
  Example: `pyxel.sounds[0].mml("T120 Q90 @1 V100 O5 L8 C4&C<G16R16>C.<G16 >C.D16 @VIB1{10,20,20} E2C2")`

- `mml(code)`<br>
  Sets the related parameters using [Music Macro Language (MML)](https://en.wikipedia.org/wiki/Music_Macro_Language). The available commands are `T`(1-900), `@`(0-3), `O`(0-4), `>`, `<`, `Q`(1-8), `V`(0-7), `X`(0-7), `L`(1/2/4/8/16/32), and `CDEFGABR`+`#+-`+`.~&`. For details on the commands, refer to [this page](docs/faq-en.md).<br>
  Example: `pyxel.sounds[0].mml("T120 Q90 @1 V100 O5 L8 C4&C<G16R16>C.<G16 >C.D16 @VIB1{10,20,20} E2C2")`

- `save(filename, sec, [ffmpeg])`<br>
  Creates a WAV file that plays the sound for the specified seconds. If FFmpeg is installed and `ffmpeg` is set to `True`, an MP4 file is also created.

- `total_sec()`<br>
  Returns the playback time of the sound in seconds. Returns `None` if infinite loop is used in MML.

### Music Class

- `seqs`<br>
  A two-dimensional list of sounds (0-63) across multiple channels

- `set(seq0, seq1, seq2, ...)`<br>
  Set the lists of sound (0-63) for each channel. If an empty list is specified, that channel will not be used for playback.<br>
  Example: `pyxel.musics[0].set([0, 1], [], [3])`

- `save(filename, sec, [ffmpeg])`<br>
  Creates a WAV file that plays the music for the specified seconds. If FFmpeg is installed and `ffmpeg` is set to `True`, an MP4 file is also created.

