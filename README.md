# Tube Decks

A two-deck party mixer for YouTube, in a single HTML file. Load tracks on two decks, crossfade between them, queue up a playlist, and let auto-mix fade each track into the next with no gaps.

It's built for fun, not for pro DJing: think "multiple YouTube tabs and juggling the volume by hand", but automated.

## Features

- **Two decks (A and B)**: load, play/pause, seek, ±10 s jumps, volume and speed per deck
- **Crossfader** with an equal-power curve, so volume doesn't dip in the middle of a fade
- **Queue**: paste one or many links at once, reorder or remove tracks
- **Auto-mix**: the next queued track is cued on the idle deck and faded in before the live track ends
- **Adjustable fade length** (1–20 seconds)
- **Mix now** button for manual transitions
- **Keyboard shortcuts**: `Space` plays/pauses the live deck, `M` mixes now
- Accepts `youtube.com`, `music.youtube.com`, `youtu.be`, `/shorts/` links or bare 11-character video IDs
- Light and dark theme, following your system setting

## Running it

### GitHub Pages

1. Put `tube-decks.html` in a repo and rename it to `index.html`.
2. In the repo, go to **Settings → Pages** and publish from your branch.
3. Open `https://<your-username>.github.io/<repo-name>/`.

### Locally

YouTube often refuses to play when the page is opened straight from disk (`file://`), so serve it instead:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000/index.html>.

## How auto-mix works

1. While a deck is playing, the first track in the queue is cued on the other deck.
2. When the live track has about *fade length* seconds left (adjusted for playback speed), the cued deck starts and the crossfader sweeps across.
3. When the fade finishes, the old deck pauses and gets the next queued track.

If a track ends early or fails to load, auto-mix moves on to the next one.

## Limitations

This uses only the official [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference), which treats each video as a black-box player. That keeps it within YouTube's terms, but it means:

- **No real DJ effects.** No EQ, filters, BPM detection, beat sync, waveforms or scratching, because the page never gets access to the raw audio.
- **Speed is limited to YouTube's own steps**, usually 0.25× to 2×. Fine pitch nudging isn't possible.
- **Videos stay visible.** YouTube's terms require the player to be shown, so there's no audio-only mode.
- **Some videos can't be embedded.** If an uploader has disabled embedding, that deck shows an error and auto-mix skips ahead.
- **Queue titles may show only the video ID.** Titles are looked up through YouTube's oEmbed endpoint, which may block requests from other sites. Each deck still shows the real title once the track loads.
- **Mobile browsers restrict autoplay**, so you may need to tap Play once on each deck before auto-mix can take over.

## Tech

Plain HTML, CSS and JavaScript in one file. No build step and no dependencies besides the YouTube IFrame API and Google Fonts.

