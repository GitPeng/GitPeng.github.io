---
layout: post
title:  "iOS 10 Project by Project 01: Speech Recognition"
date:   2016-09-20 20:00:00
categories: tutorial
---

Speech is a new framework introduced in iOS 10 which gives the app the ability of speech recognition. With the help of the framework, we can transcribe the real-time or recorded audio into text. So we can imagine a note app which translates the user's voice to text or a Siri-like app which takes the user's voice input as command.

### Request permissions

To use speech recognition, we need to request user's permission.

In our app's Info.plist, add a new key NSSpeechRecognitionUsageDescription to tell the user why we
need the speech recognition.

Then request permission with codes like

<script src="https://gist.github.com/NilStack/07c8459b9a5bb7f4d83f5e8ed3e92257.js"></script>


### Recorded audio

Relatively speaking，recorded audio is easier to handle.

<script src="https://gist.github.com/NilStack/36ba6e12e11ccf0d12cf052c698abb50.js"></script>

Here is an example project [SpeechRecognition](https://github.com/zats/SpeechRecognition/).

### Real-time voice

The codes are from [shu223/iOS-10-Sampler](https://github.com/shu223/iOS-10-Sampler).

#### Set up recorder

    let audioSession = AVAudioSession.sharedInstance()
    try audioSession.setCategory(AVAudioSessionCategoryRecord)
    try audioSession.setMode(AVAudioSessionModeMeasurement)
    try audioSession.setActive(true, with: .notifyOthersOnDeactivation)

#### Set up speech recognizer

    let speechRecognizer = SFSpeechRecognizer(locale: Locale.current)!
    speechRecognizer.delegate = self

    recognitionRequest = SFSpeechAudioBufferRecognitionRequest()
    guard let recognitionRequest = recognitionRequest else { fatalError("Unable to created a     SFSpeechAudioBufferRecognitionRequest object") }
    recognitionRequest.shouldReportPartialResults = true

    recognitionTask = speechRecognizer.recognitionTask(with: recognitionRequest) { result, error in
            var isFinal = false

            if let result = result {
                self.textView.text = result.bestTranscription.formattedString
                isFinal = result.isFinal
            }

            if error != nil || isFinal {
                self.audioEngine.stop()
                inputNode.removeTap(onBus: 0)

                self.recognitionRequest = nil
                self.recognitionTask = nil

            }
        }

        let recordingFormat = inputNode.outputFormat(forBus: 0)
        inputNode.installTap(onBus: 0, bufferSize: 1024, format: recordingFormat) { (buffer: AVAudioPCMBuffer, when: AVAudioTime) in
            self.recognitionRequest?.append(buffer)
        }

#### Start the recorder

        audioEngine.prepare()
        try audioEngine.start()
