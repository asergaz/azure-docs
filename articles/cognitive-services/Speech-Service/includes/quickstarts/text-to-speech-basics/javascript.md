---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 03/15/2022
ms.author: eur
---

[!INCLUDE [Header](../../common/javascript.md)]

[!INCLUDE [Introduction](intro.md)]

## Prerequisites

[!INCLUDE [Prerequisites](../../common/azure-prerequisites.md)]

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=JAVASCRIPT&Pillar=Speech&Product=text-to-speech&Page=quickstart&Section=Prerequisites" target="_target">I ran into an issue</a>

## Set up the environment

Before you can do anything, you need to install the Speech SDK for JavaScript. If you just want the package name to install, run `npm install microsoft-cognitiveservices-speech-sdk`. For guided installation instructions, see the [SDK installation guide](../../../quickstarts/setup-platform.md?pivots=programming-language-javascript).

### Set environment variables

[!INCLUDE [Environment variables](../../common/environment-variables.md)]

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=JAVASCRIPT&Pillar=Speech&Product=text-to-speech&Page=quickstart&Section=Set-up-the-environment" target="_target">I ran into an issue</a>

## Synthesize to file output

Follow these steps to create a Node.js console application for speech synthesis.

1. Open a command prompt where you want the new project, and create a new file named `SpeechSynthesis.js`.
1. Install the Speech SDK for JavaScript:
    ```console
    npm install microsoft-cognitiveservices-speech-sdk
    ```
1. Copy the following code into `SpeechSynthesis.js`:

    ```javascript
    (function() {

        "use strict";
        
        var sdk = require("microsoft-cognitiveservices-speech-sdk");
        var readline = require("readline");
        
        var audioFile = "YourAudioFile.wav";
        // This example requires environment variables named "SPEECH_KEY" and "SPEECH_REGION"
        const speechConfig = sdk.SpeechConfig.fromSubscription(process.env.SPEECH_KEY, process.env.SPEECH_REGION);
        const audioConfig = sdk.AudioConfig.fromAudioFileOutput(audioFile);
        
        // The language of the voice that speaks.
        speechConfig.speechSynthesisVoiceName = "en-US-JennyNeural"; 
        
        // Create the speech synthesizer.
        var synthesizer = new sdk.SpeechSynthesizer(speechConfig, audioConfig);
      
        var rl = readline.createInterface({
          input: process.stdin,
          output: process.stdout
        });
      
        rl.question("Enter some text that you want to speak >\n> ", function (text) {
          rl.close();
          // Start the synthesizer and wait for a result.
          synthesizer.speakTextAsync(text,
              function (result) {
            if (result.reason === sdk.ResultReason.SynthesizingAudioCompleted) {
              console.log("synthesis finished.");
            } else {
              console.error("Speech synthesis canceled, " + result.errorDetails +
                  "\nDid you set the speech resource key and region values?");
            }
            synthesizer.close();
            synthesizer = null;
          },
              function (err) {
            console.trace("err - " + err);
            synthesizer.close();
            synthesizer = null;
          });
          console.log("Now synthesizing to: " + audioFile);
        });
    }());
    ```

1. In `SpeechSynthesis.js`, optionally you can rename `YourAudioFile.wav` to another output filename.

1. To change the speech synthesis language, replace `en-US-JennyNeural` with another [supported voice](~/articles/cognitive-services/speech-service/supported-languages.md#prebuilt-neural-voices). All neural voices are multilingual and fluent in their own language and English. For example, if the input text in English is "I'm excited to try text to speech" and you set `es-ES-ElviraNeural`, the text is spoken in English with a Spanish accent. If the voice does not speak the language of the input text, the Speech service won't output synthesized audio.

Run your new console application to start speech synthesis to a file:

```console
node.exe SpeechSynthesis.js
```

> [!IMPORTANT]
> Make sure that you set the `SPEECH__KEY` and `SPEECH__REGION` environment variables as described [above](#set-environment-variables). If you don't set these variables, the sample will fail with an error message.

The provided text should be output to an audio file:

```console
Enter some text that you want to speak >
> I'm excited to try text to speech
Now synthesizing to: YourAudioFile.wav
synthesis finished.
```

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=JAVASCRIPT&Pillar=Speech&Product=text-to-speech&Page=quickstart&Section=Synthesize-to-file-output" target="_target">I ran into an issue</a>

> [!WARNING]
> There is a known issue on Windows 11 that might affect some types of Secure Sockets Layer (SSL) and Transport Layer Security (TLS) connections. For more information, see the [troubleshooting guide](/azure/cognitive-services/speech-service/troubleshooting#connection-closed-or-timeout).

## Remarks
Now that you've completed the quickstart, here are some additional considerations:

This quickstart uses the `SpeakTextAsync` operation to synthesize a short block of text that you enter. You can also get text from files as described in these guides:
- For information about speech synthesis from a file, see [How to synthesize speech](~/articles/cognitive-services/speech-service/how-to-speech-synthesis.md) and [Improve synthesis with Speech Synthesis Markup Language (SSML)](~/articles/cognitive-services/speech-service/speech-synthesis-markup.md).
- For information about batch synthesis, see [Synthesize long-form text to speech](~/articles/cognitive-services/speech-service/long-audio-api.md). 

## Clean up resources

[!INCLUDE [Delete resource](../../common/delete-resource.md)]

