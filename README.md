# SoundPad Application

## 🎵 Introduction

SoundPad is a versatile, web-based audio player designed for live events, streams, or any scenario requiring dynamic sound cue management. It allows users to load, play, and control multiple audio tracks simultaneously through an intuitive grid interface. The application is highly configurable via a JSON file, supporting complex chained audio events through a powerful macro system.

## ✨ Features

### Dynamic Sound Loading
- Load sounds from local audio files or a sound-config.json file
- Automatic configuration detection on startup
- Manual file selection for both audio and JSON configurations

### Master Controls
- Global master volume slider (0-100%)
- "⏹️ STOP ALL" button for immediate stopping
- "🎭 FADE OUT ALL" button for smooth fade-out

### Individual Sound Controls
- **Play/Stop**: Basic playback control
- **Volume**: Adjust individual sound volume (0-100%)
- **Pan**: Precise stereo panning control (-10 to +10) with visual indicators
- **Loop**: Toggle seamless looping with UI indicators
- **Seek Bar**: Visual progress bar with current/duration time display and seeking
- **Fade Out**: Smooth 1-second fade to silence
- **EQ**: Apply "telephone effect" EQ using Web Audio API filters (low/high shelf)

### Currently Playing Sidebar
- Real-time list of active sounds with playback status
- Visual indicators for looping status (color-coded borders)
- One-click stop buttons for individual tracks
- Automatic updates during playback

### Advanced Macro System
- Define on_play and on_stop triggers in JSON configuration
- Chainable actions: play, stop, fadeout, stopAll, fadeOutAll, toggleEQ, setVolume
- Execution guards prevent infinite macro loops
- Macro actions execute in sequence with 10ms delays

## 🚀 Getting Started

### File Structure
Place your index.html, sound-config.json, and a sound folder (containing your audio files) in the same directory.

```
/your-project
├── index.html
├── sound-config.json
└── /sound
    ├── sound1.mp3
    ├── sound2.wav
    └── ...
```

### Open the Application
Open the index.html file in a modern web browser.

### Loading Sounds
- **Automatic**: The application will automatically try to load sounds from sound-config.json on startup
- **Manual JSON**: Click the second "Choose File" button to load a different .json configuration file
- **Manual Audio Files**: Click the first "Choose File" button to load one or more audio files directly. Their names will be generated from the filenames

## ⚙️ Configuration (sound-config.json)

The core of the SoundPad's power lies in its JSON configuration. The file should contain a single object with a sounds key, which is an array of sound objects.

### Basic Sound Object
Each object in the sounds array can have the following properties:

| Key | Type | Required | Description | Example |
|-----|------|----------|-------------|---------|
| name | String | Yes | The display name of the sound. Must be unique. | "Canteen BGM" |
| url | String | Yes | The relative path to the audio file. | "sound/Canteen BGM.mp3" |
| volume | Number | No | The initial volume (0-100). Defaults to 100. | 80 |
| pan | Number | No | The initial stereo pan (-10 to 10). Defaults to 0 (center). | -2 (Slightly left) |
| loop | Boolean | No | Whether the sound should loop. Defaults to false. | true |
| on_play | Array | No | An array of macro objects to execute when this sound plays. | [{"action": "play", ...}] |
| on_stop | Array | No | An array of macro objects to execute when this sound stops. | [{"action": "stop", ...}] |

### Example sound-config.json:
```json
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
```

## 🔗 Macro System

The macro system lets you automate actions. By adding on_play or on_stop arrays to a sound object, you can trigger a sequence of events.

Each macro is an object in the array with an action and often a target.

### Available Macro Actions

| Action | Target Required | Value Required | Description | Example |
|--------|----------------|----------------|-------------|---------|
| play | Yes | No | Plays the target sound. | {"action": "play", "target": "MUSIC DJ"} |
| stop | Yes | No | Stops the target sound. | {"action": "stop", "target": "MUSIC DJ"} |
| fadeout | Yes | No | Fades out the target sound over 1 second. | {"action": "fadeout", "target": "Canteen BGM"} |
| stopAll | No | No | Stops all currently playing sounds immediately. | {"action": "stopAll"} |
| fadeOutAll | No | No | Fades out all currently playing sounds. | {"action": "fadeOutAll"} |
| toggleEQ | Yes | No | Toggles the EQ effect for the target sound. | {"action": "toggleEQ", "target": "Canteen BGM"} |
| setVolume | Yes | Yes (value) | Sets the volume of the target sound. value is 0-100. | {"action": "setVolume", "target": "Canteen BGM", "value": 25} |

### Chaining Example
You can create complex sequences. For example, when "PHONE CALL" finishes, it plays "GINA 1". When "GINA 1" plays, it automatically lowers the volume of the "Canteen BGM".

```json
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
```

## 🛠️ Technology Used

- **HTML5 Audio Elements** with MediaElementSource
- **CSS3** (Flexbox, Grid, Custom Checkboxes, Gradient Backgrounds)
- **JavaScript** (ES6+ Modules, Async Operations)
- **Web Audio API** (AudioContext, StereoPannerNode, BiquadFilterNode)
- **JSON Configuration** with Macro Chaining
- **RequestAnimationFrame** for Smooth Animations
