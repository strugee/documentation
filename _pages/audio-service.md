---
ID: 33177
post_title: Audio Service
author: Kathy Reid
post_excerpt: ""
layout: page
permalink: >
  http://mycroft.ai/documentation/skills/audio-service/
published: true
post_date: 2017-12-03 01:42:34
---
# Audio Service

- [Audio Service](#audio-service)
  * [How to set up the Audio Service](#how-to-set-up-the-audio-service)
  * [Starting playback](#starting-playback)
  * [More technical information](#more-technical-information)
  * [The backends](#the-backends)
  * [PulseAudio features](#pulseaudio-features)
  * [AudioService Technical Documentation](#audioservice-technical-documentation)

The audio service handles playback and queueing of tracks. The `mycroft-core` distribution of Mycroft includes a _Playback Skill_ which can be used to control playback after it has been started. This means that playback only needs to be started in the **Skill**. Controlling playback can then be done through the _Playback Skill_.

## How to set up the Audio Service

First, import the

`AudioService`

 class.

```python
from mycroft.skills.audioservice import AudioService
```

Then in the `initialize()` method of your **Skill**, instantiate an `AudioService` object:

```python
    def initialize(self):
        self.audio_service = AudioService(self.bus)

        #Other initialize code
        [...]
```

## Starting playback

Once the `AudioService` instance is created, you can start playback by simply calling the `play()` method with a track URI:

```python
        self.audio_service.play('file:///path/to/my/track.mp3')
```
@TODO what URI schemas are supported?

or with a list of tracks

```python
        self.audio_service.play(['file:///path/to/my/track.mp3', 'http://tracks-online.com/my/track.mp3'])
```

The play method has an optional second argument to further process the user's **Utterance**. Currently this can only be used to select backend (where you want to send the audio) but in the future it will be able to handle requests like

 > Hey Mycroft, play Hello Nasty by the Beastie Boys at half volume. We don't want to wake the neighbours

To use this feature the **Utterance** received from the intent service must be passed

```python
    def play_playlist_handler(self, message):
        self.audioservice.play(PLAYLIST, message.data['utterance'])
```

## More technical information

## The backends

The default backend is still mpg123 for mp3 files which is very limited but is the most generally available or multiple platforms.

Included in this release there's also

- `VLC` (a very general purpose media player)
- `mopidy` (a common audio server in the Raspberry Pi community)
- `chromecast` (experimental)

These haven't been extensively tested on the Mark 1 yet.

## PulseAudio features

The audio service hooks into the PulseAudio controls and can mute playback of sound streams beyond Mycroft's control. This is currently deactivated by default but can be enabled by changing the `mycroft.conf` configuration found in `mycroft/configuration/mycroft.conf`

```json
  "play_wav_cmdline": "paplay %1 --stream-name=mycroft-voice",
  "Audio": {
    "pulseaudio": "mute"
  }
```

## AudioService Technical Documentation
More information on AudioService methods can be found in the [Mycroft Technical Documentation](https://mycroft-core.readthedocs.io/en/master/source/mycroft.html#audioservice-class).