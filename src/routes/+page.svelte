<script>
  import * as ort from "onnxruntime-web";
  import { onMount } from "svelte";
  async function main() {
    //load vad model
    try {
      const session = await ort.InferenceSession.create("model.onnx");
    //   console.log(session);
    } catch (e) {
      document.write(`failed to inference ONNX model: ${e}.`);
    }

    const mediaStreamConstraints = {
       audio: {
         sampleRate: 16000,
       },
       video: false
   }

    // mic and audio
    navigator.mediaDevices
      .getUserMedia(mediaStreamConstraints)
      .then(function (stream) {
        // Create a new MediaRecorder instance and pass in the audio stream
        const mediaRecorder = new MediaRecorder(stream);
        let chunks = [];
        // Listen for data available event and push the audio data to the chunks array
        mediaRecorder.ondataavailable = function (event) {
          console.log("event.data");
          chunks.push(event.data);
        };

        // When recording is stopped, log the audio chunks to the console
        // mediaRecorder.onstop = function () {
        //   const audioBlob = new Blob(chunks, { type: "audio/wav" });
        //   const audioUrl = URL.createObjectURL(audioBlob);
        //   console.log("Blob URL:", audioUrl);

        //   //binary audio data
        //   const reader = new FileReader();
        //     reader.onloadend = function() {
        //         const rawAudioBuffer = reader.result;
        //         const adjustedLength = Math.floor(rawAudioBuffer.byteLength / 4) * 4;
        //         const audioArray = new Float32Array(rawAudioBuffer.slice(0, adjustedLength));
        //         console.log('Raw Audio Array:', audioArray);
        //     };
        //     reader.readAsArrayBuffer(audioBlob);
        // };
        //


        // Start recording
        mediaRecorder.start();

        // Stop recording after 5 seconds (for example)
        // setTimeout(function () {
        //   mediaRecorder.stop();
        // }, 5000);
      })
      .catch(function (err) {
        console.log("The following error occurred: " + err);
      });
  }
  onMount(() => {
    main();
  });
</script>

<h1>Hello</h1>
