# Misc. Development

## Code Counting <a href="#bash-stuff" id="bash-stuff"></a>

Use [tokei](https://github.com/XAMPPRocky/tokei#how-to-use-tokei).

```
tokei ./* --exclude --exclude '**/*.html' --exclude '**/*.json'
```

### GIF Creation for documentation

Use [asciinema](https://asciinema.org/) and [asciicast2gif](https://github.com/asciinema/asciicast2gif/).

```
asciinema rec -i 2.5 cloudsplaining.cast
asciicast2gif -t solarized-dark cloudsplaining.cast cloudsplaining.gif
```

When converting Quicktime movies to GIFs:

```
ffmpeg -i cloudsplaining.mov -s 600x400 -pix_fmt rgb24 -r 10 -f gif - | gifsicle --optimize=3 --delay=3 > cloudsplaining-report.gif
```

### Convert RST to MD One-liner

```
for rst in **/*.rst; do pandoc "$rst" -f rst -t markdown -o "${rst%.*}.md"; done
```
