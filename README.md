
# omx-layers (Node.js)
An interface for Node.js allowing you to layer multiple [omxplayer](https://github.com/popcornmix/omxplayer) instances and control them via D-Bus.

# How to Install
```
npm install omx-layers
```
# Requirements
Remember that the `omxplayer` only works on the Raspberry Pi (and similar hardware?). Performance is great on the Pi 3.

`omxplayer` is installed by default on the "Raspbian with Desktop" version of [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) but if you have installed the "Lite" version (console only) then you might need to install it manually:
```
sudo apt-get install omxplayer
```


# Examples
## Single layer only
```
const omx = require('omx-layers');

let player = new omx({
	audioOutput: 'local',
	blackBackground: true,
	disableKeys: true,
	disableOnScreenDisplay: true,
});
```
Then play a clip like this:
```
player.open('myclip.mp4', () => {
	console.log('playback finished!');
});
```

## Multiple players, multiple layers
```
const omx = require('omx-layers');
let players = [];
const numPlayers = 2;

for (var i=0; i<numPlayers; i++) {
	players.push(
		new omx({
			audioOutput: 'local',
			blackBackground: true,
			disableKeys: true,
			disableOnScreenDisplay: true,
			layer: i+1
		})
	);
}

```
Find the clip with the layer you want, and play:
```
// Let's say you wanted to play a clip on layer 2...
if (player[1].getLayer() == 2) {
	player[1].open('foreground-clip.mp4');
}
```

# Options
* `audioOutput`: 'hdmi' | 'local' | 'both'
* `blackBackground`: boolean, true by default
* `layer`: 1-infinity (2 is probably enough!); if omitted then clips will automatically player on layer 1
* `disableKeys`: boolean, false by default
* `disableOnScreenDisplay`:  boolean, false by default


# Properties
## Get duration of current track/movie in seconds
``omx.getCurrentDuration();``

## Get position of current track/movie in seconds
``omx.getCurrentPosition();``

This function can be called many times per second without bothering the DBus since the position is extrapolated from the short term cached paying status.

## Get volume as fraction of max (0.0 - 1.0)
``omx.getCurrentVolume();``

# Methods

## Jump to point in file/seek relative to current position (-Inf to +Inf)
``omx.seek(seconds);``

## Jump to point in file/seek relative to start point (absolute)
``omx.setPosition(seconds);``

## Stop playing
``omx.stop();``

## Quit omxplayer
``omx.quit();``

## Pause omxplayer
``omx.pause();``

Note: Unlike hitting the spacebar, this method pauses only when playing and remains paused when allready paused.

## Resume playing
``omx.play();``

Note: Unlike hitting the spacebar, this method starts playing only when paused and remains playing when allready playing.

## Toggle pause/play
``omx.togglePlay();``

Note: Same function as hitting spacebar in omxplayer.

## Volume up
``omx.volumeUp();``

Note: Same function as "+" key in omxplayer.

## Volume down
``omx.volumeDown();``

Note: Same function as "-" key in omxplayer.

## Set volume to a fraction of the max volume (0.0 - 1.0)
``omx.setVolume(vol);``
