# nanoflux-plugin-youtube

A nanoflux external plugin for YouTube channel feeds. It reads the channel's
recent videos through YouTube's internal browse API, which carries duration and
view counts that the channel RSS omits.

## Install

```bash
cd nanoflux/plugins/youtube
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o ../nanoflux-plugin-youtube .
```

Restart nanoflux after putting the binary in its plugins directory
(`NF_PLUGINS_DIR`, `/plugins` in the container).

## Usage

Add a channel URL, for example `https://www.youtube.com/@SomeChannel`.
