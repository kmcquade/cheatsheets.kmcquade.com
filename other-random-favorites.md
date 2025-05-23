---
description: >-
  These don't fit neatly into a particular category but I want quick access to
  them.
---

# Other random favorites

#### GitHub Markdown Warning Panels, Warning Boxes, Notes, etc.

[https://gist.github.com/cseeman/8f3bfaec084c5c4259626ddd9e516c61](https://gist.github.com/cseeman/8f3bfaec084c5c4259626ddd9e516c61)

Also covered here: [https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts)

#### /dev/null usage - to hide output of a command

```
az login > /dev/null 2>&1
```

####  asciinema

```
asciinema rec -i 2.5 cloudsplaining.cast
```

### Recording GIF Demos

* Record with Zoom or Quicktime Player
* Trim on Quicktime
* Install prerequisites

```
brew install gifsicle ffmpeg
```

* Quickly convert the `.mov` file to a GIF using this:

```
export INPUT_FILE=input.mov
export OUTPUT_FILE=output.gif
# set frame rate to 3 for slow, 10 for fast
export FRAME_RATE=3

ffmpeg -i $INPUT_FILE -pix_fmt rgb24 -r $FRAME_RATE -f gif - \
    | gifsicle --optimize=3 --delay=3 > $OUTPUT_FILE
```

