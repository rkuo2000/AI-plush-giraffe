# AMB82-Mini Smart Giraffe Plush Toy

## Project Proposal

### 1. Project Overview

This project converts a giraffe plush toy into an interactive AI
companion using the Realtek AMB82-Mini as the embedded controller. The
toy can use a camera, microphone, speaker, Wi-Fi, local AI inference,
and optional cloud AI services to see, hear, speak, recognize simple
situations, and react to its environment.

The design keeps the plush exterior soft while placing the electronics
inside a removable internal module for development and maintenance.

### 2. Objectives

-   Add visual perception using the AMB82-Mini camera interface.
-   Add voice interaction using microphone input, speech recognition,
    and text-to-speech.
-   Run lightweight computer-vision models locally, such as person,
    face-presence, gesture, or object detection.
-   Use Wi-Fi to optionally connect to an LLM/VLM/STT/TTS service for
    richer conversations.
-   Add simple physical responses using LEDs and/or small servos.
-   Store configuration, sound effects, and interaction logs on microSD.
-   Provide a modular platform for education and embedded-AI
    experiments.

### 3. Proposed User Experience

A user approaches the giraffe and says a wake phrase or presses a
concealed button. The giraffe can turn its head slightly toward the
user, detect a person with its camera, listen to a question, and answer
through its speaker. It can also run modes such as storytelling,
vocabulary practice, object recognition, quizzes, and simple interactive
games.

### 4. System Architecture

``` text
                         Wi-Fi
                           |
                  +------------------+
                  | Cloud AI Service |
                  | LLM/VLM/STT/TTS  |
                  +--------+---------+
                           |
                           v
+---------+       +-------------------+       +---------+
| Camera  | ----> |    AMB82-Mini     | ----> | Speaker |
+---------+       |                   |       +---------+
                  | Vision / Audio AI |
+---------+ ----> | State Machine     | ----> LEDs
| Mic     |       | Wi-Fi / microSD   |
+---------+       +---------+---------+
                            |
                            v
                    Servo Controller
                            |
                    Head / ear motion
```

A useful hybrid design is **local-first perception + optional cloud
reasoning**. Fast events such as button presses, wake detection, simple
vision inference, LEDs, and servo motion stay on the toy. More
computationally demanding language or multimodal reasoning can be sent
through Wi-Fi when enabled.

### 5. Hardware

  Module                             Purpose
  ---------------------------------- -----------------------------------------
  AMB82-Mini                         Main controller and local AI processing
  Compatible camera                  Person/object/gesture recognition
  MEMS/I2S microphone                Voice capture
  Small speaker + amplifier          Speech and sound effects
  microSD card                       Audio, configuration, logs, AI assets
  1--2 micro servos                  Optional head/ear movement
  RGB LEDs                           Status and expressive feedback
  Push button                        Wake/reset/mode selection
  Rechargeable battery pack          Portable power
  Power regulator/charging circuit   Stable system power

Exact microphone, amplifier, battery, and servo interfaces should be
selected only after checking the AMB82-Mini pinout, voltage/current
limits, and the chosen peripherals.

### 6. Mechanical Modification

A removable electronics enclosure can be installed in the torso. The
camera can be concealed near the chest, neck, or head behind a suitable
opening. The microphone should have an acoustically transparent opening,
while the speaker can face a perforated or mesh-covered area.

Servos should be mounted to an internal frame rather than directly to
plush fabric. Keep rigid components, wiring, batteries, and moving
linkages inaccessible from the outside. The battery compartment should
be serviceable by an adult, and the prototype should not be treated as a
normal washable plush toy.

### 7. Software Architecture

``` text
Boot
 |
 +--> Hardware initialization
 +--> Load configuration/model
 +--> Wi-Fi initialization (optional)
 |
 v
IDLE
 |
 +-- wake/button/person event --> LISTEN
                                  |
                                  v
                              UNDERSTAND
                         local command / STT
                                  |
                                  v
                               RESPOND
                         local action / LLM
                                  |
                  +---------------+---------------+
                  |                               |
                SPEAK                           ACT
             TTS/audio                    LEDs / servo
                  |                               |
                  +---------------+---------------+
                                  |
                                  v
                                 IDLE
```

Suggested firmware modules:

``` text
src/
  main.cpp
  camera.cpp
  vision.cpp
  audio_input.cpp
  speech.cpp
  ai_client.cpp
  motion.cpp
  leds.cpp
  storage.cpp
  config.cpp
```

### 8. AI Functions

**Phase 1 --- Offline interaction** - Person/object detection -
Button-triggered prerecorded speech - LED expressions - Simple servo
gestures

**Phase 2 --- Voice assistant** - Wake interaction - Speech-to-text -
Command classification - Text-to-speech - Basic conversation state

**Phase 3 --- Multimodal companion** - Camera snapshot + user question -
VLM scene/object description - LLM-generated educational responses -
Context-aware movements and sound

For a child-facing prototype, avoid silently recording or uploading
continuous audio/video. Make networked capture obvious, minimize stored
data, and provide a hardware or clearly visible software method to
disable camera/microphone functions.

### 9. Example Interaction

``` text
User:   "Hello, Giraffe."
Toy:    [LED animation + head movement]
Toy:    "Hi! What would you like to learn?"

User:   "What am I holding?"
Toy:    [captures an image]
        [local detector or VLM analyzes it]
Toy:    "It looks like a book."
```

### 10. Development Milestones

  Stage   Deliverable
  ------- --------------------------------------------------------------
  1       AMB82-Mini boots and controls status LED
  2       Camera capture and local vision demo
  3       Microphone input and speaker playback
  4       Servo/LED expression controller
  5       Wi-Fi API client
  6       STT → reasoning → TTS conversation pipeline
  7       Integrate electronics into removable toy module
  8       Power, thermal, privacy, mechanical, and interaction testing

### 11. Success Criteria

The prototype should boot reliably, detect selected visual targets,
capture intelligible speech, play understandable responses, perform safe
limited motion, recover gracefully when Wi-Fi is unavailable, and allow
electronics to be removed for maintenance.

------------------------------------------------------------------------

## Repository README

### Recommended Repository Layout

``` text
AMB82-Smart-Giraffe/
├── README.md
├── firmware/
│   ├── main/
│   └── modules/
├── models/
├── audio/
├── config/
├── docs/
│   ├── architecture.md
│   ├── wiring.md
│   └── assembly.md
└── assets/
    └── plush_toy_reference.webp
```

### Getting Started

1.  Prepare the AMB82-Mini development environment using the current
    board documentation.
2.  Test camera capture before installing the board inside the toy.
3.  Test microphone recording and speaker playback independently.
4.  Add the local vision model and verify memory/performance.
5.  Connect Wi-Fi and configure optional AI endpoints.
6.  Test servo motion with conservative travel limits before mechanical
    installation.
7.  Integrate all components into a removable internal chassis.
8.  Run extended power and temperature tests before normal use.

### Configuration Example

``` text
WIFI_SSID=your_network
WIFI_PASSWORD=your_password

AI_MODE=hybrid
STT_ENDPOINT=...
LLM_ENDPOINT=...
TTS_ENDPOINT=...

SERVO_ENABLED=true
CAMERA_ENABLED=true
```

Do not commit API keys or Wi-Fi credentials to a public repository.
Store secrets outside version control and add local secret/configuration
files to `.gitignore`.

### Main Control Loop --- Pseudocode

``` cpp
void loop() {
    Event event = detectEvent();

    if (event == WAKE_EVENT) {
        setExpression(LISTENING);
        Audio audio = recordUtterance();

        Intent intent = understand(audio);

        if (intent.requiresVision) {
            Image frame = captureImage();
            intent.attach(frame);
        }

        Response response = processIntent(intent);

        performMotion(response.motion);
        speak(response.text);
        setExpression(IDLE);
    }
}
```

### Suggested First Prototype

For the first working version, keep the scope small:

``` text
Camera -> person detection
Button -> trigger interaction
microSD -> prerecorded phrases
Speaker -> audio response
RGB LED -> state feedback
Servo -> one head movement
```

After this is stable, add STT, TTS, LLM, and VLM features one at a time.

### Safety Notes

-   Verify voltage compatibility before connecting peripherals.
-   Size the power supply for peak board, audio-amplifier, and servo
    current.
-   Do not power a servo from a logic pin.
-   Use mechanical travel limits for moving parts.
-   Prevent access to batteries, wiring, and small components.
-   Monitor battery and regulator temperature during testing.
-   Keep the electronics module removable; disconnect power before
    servicing.
-   Treat the modified toy as an engineering prototype rather than a
    certified children's product.

### Future Extensions

Possible extensions include gesture recognition, educational
Mandarin/English modes, object-learning games,
parent/teacher-configurable lesson content, offline command recognition,
animated ears, and a companion web dashboard.

### License

Choose a license appropriate for the firmware, documentation, models,
and third-party components used in the project.
