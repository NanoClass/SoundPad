SoundPad Application
🎵 Introduction
SoundPad is a versatile, web-based audio player designed for live events, streams, or any scenario requiring dynamic sound cue management. It allows users to load, play, and control multiple audio tracks simultaneously through an intuitive grid interface. The application is highly configurable via a JSON file, supporting complex chained audio events through a powerful macro system.
✨ Features
Dynamic Sound Loading: Load sounds from local audio files or a sound-config.json file.
Master Controls: A global master volume slider and "Stop All" / "Fade Out All" buttons for immediate control over all playing audio.
Individual Sound Controls: Each sound has its own set of controls:
Play/Stop: Basic playback control.
Volume: Adjust individual sound volume (0-100%).
Pan: Adjust stereo panning from left to right.
Loop: Toggle seamless looping for any sound.
Seek Bar: A visual progress bar that shows playback time and allows seeking.
Fade Out: Smoothly fades a sound to silence before stopping.
EQ: Apply a pre-configured "telephone effect" equalizer.
"Currently Playing" Sidebar: A real-time list of all active sounds, with quick access to stop individual tracks.
Advanced Macro System: Define on_play and on_stop triggers within the JSON configuration to chain actions, creating complex audio sequences automatically.
🚀 Getting Started
File Structure: Place your index.html, sound-config.json, and a sound folder (containing your audio files) in the same directory.
/your-project
├── index.html
├── sound-config.json
└── /sound
    ├── sound1.mp3
    ├── sound2.wav
    └── ...



Open the Application: Open the index.html file in a modern web browser.
Loading Sounds:
Automatic: The application will automatically try to load sounds from sound-config.json on startup.
Manual JSON: Click the second "Choose File" button to load a different .json configuration file.
Manual Audio Files: Click the first "Choose File" button to load one or more audio files directly. Their names will be generated from the filenames.
⚙️ Configuration (sound-config.json)
The core of the SoundPad's power lies in its JSON configuration. The file should contain a single object with a sounds key, which is an array of sound objects.
Basic Sound Object
Each object in the sounds array can have the following properties:
|
| Key | Type | Required | Description | Example |
| name | String | Yes | The display name of the sound. Must be unique. | "Canteen BGM" |
| url | String | Yes | The relative path to the audio file. | "sound/Canteen BGM.mp3" |
| volume | Number | No | The initial volume (0-100). Defaults to 100. | 80 |
| pan | Number | No | The initial stereo pan (-10 to 10). Defaults to 0 (center). | -2 (Slightly left) |
| loop | Boolean | No | Whether the sound should loop. Defaults to false. | true |
| on_play | Array | No | An array of macro objects to execute when this sound plays. | [{"action": "play", ...}] |
| on_stop | Array | No | An array of macro objects to execute when this sound stops. | [{"action": "stop", ...}] |
Example sound-config.json:
{
  "sounds": [
    {
      "name": "Canteen BGM",
      "url": "sound/Canteen BGM.mp3",
      "volume": 80,
      "pan": -2,
      "loop": true
    },
    {
      "name": "Phone Call",
      "url": "sound/CALL.mp3",
      "volume": 90,
      "loop": false,
      "on_stop": [
        { "action": "play", "target": "GINA 1" }
      ]
    },
    {
      "name": "GINA 1",
      "url": "sound/GINA 1.mp3",
      "volume": 90,
      "loop": false,
      "on_play": [
        { "action": "setVolume", "target": "Canteen BGM", "value": 25 }
      ]
    }
  ]
}



🔗 Macro System
The macro system lets you automate actions. By adding on_play or on_stop arrays to a sound object, you can trigger a sequence of events.
Each macro is an object in the array with an action and often a target.
Available Macro Actions
| Action | Target Required | Value Required | Description | Example |
| play | Yes | No | Plays the target sound. | {"action": "play", "target": "MUSIC DJ"} |
| stop | Yes | No | Stops the target sound. | {"action": "stop", "target": "MUSIC DJ"} |
| fadeout | Yes | No | Fades out the target sound over 1 second. | {"action": "fadeout", "target": "Canteen BGM"} |
| stopAll | No | No | Stops all currently playing sounds immediately. | {"action": "stopAll"} |
| fadeOutAll | No | No | Fades out all currently playing sounds. | {"action": "fadeOutAll"} |
| toggleEQ | Yes | No | Toggles the EQ effect for the target sound. | {"action": "toggleEQ", "target": "Canteen BGM"} |
| setVolume | Yes | Yes (value) | Sets the volume of the target sound. value is 0-100. | {"action": "setVolume", "target": "Canteen BGM", "value": 25} |
Chaining Example
You can create complex sequences. For example, when "PHONE CALL" finishes, it plays "GINA 1". When "GINA 1" plays, it automatically lowers the volume of the "Canteen BGM".
sound-config.json excerpt:
{
    "name": "PHONE CALL",
    "url": "sound/CALL.mp3",
    "on_stop": [
        { "action": "play", "target": "GINA 1" }
    ]
},
{
    "name": "GINA 1",
    "url": "sound/GINA 1.mp3",
    "on_play": [
        { "action": "setVolume", "target": "Canteen BGM", "value": 25 }
    ]
}



🛠️ Technology Used
HTML5
CSS3 (Flexbox, Grid)
JavaScript (ES6+)
Web Audio API (for advanced features like Panning and EQ)
