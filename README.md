# AI-plush-giraffe

## [Proposal](https://github.com/rkuo2000/AI-plush-giraffe/blob/main/PROPOSAL.md)
### Objectives:
* Add visual perception using the AMB82-Mini camera interface.
* Add voice interaction using microphone input, speech recognition, and text-to-speech.
* Run lightweight computer-vision models locally, such as person, face-presence, gesture, or object detection.
* Use Wi-Fi to optionally connect to an LLM/VLM/STT/TTS service for richer conversations.
* Add simple physical responses using LEDs and/or small servos.
* Store configuration, sound effects, and interaction logs on microSD.
* Provide a modular platform for education and embedded-AI experiments.

## System Block Diagram
![](https://github.com/rkuo2000/AI-plush-giraffe/blob/main/assets/AI-plush-toy_block_diagram.png?raw=true)

### LLM 
`gemma4:e2b` based on [Goole-AI-Edge Gallery](https://github.com/google-ai-edge/gallery) v1.0.19 <br>

### Hardware : 
1. EVB : AMB82-Mini (built-in camera & mic)
2. Sound : PAM8403 + speaker
   
## ProtoTyping
![](https://github.com/rkuo2000/AI-plush-giraffe/blob/main/assets/plush_toy_giraffe.webp?raw=true)
