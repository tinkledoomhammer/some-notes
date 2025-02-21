


### General audio graph definition

General containers and definitions that shape audio graphs in Web Audio API usage.

[`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext "The AudioContext interface represents an audio-processing graph built from audio modules linked together, each represented by an AudioNode.")

The  **`AudioContext`**  interface represents an audio-processing graph built from audio modules linked together, each represented by an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:"). An audio context controls the creation of the nodes it contains and the execution of the audio processing, or decoding. You need to create an  `AudioContext`  before you do anything else, as everything happens inside a context.

[`AudioContextOptions`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContextOptions "The AudioContextOptions dictionary is used to specify configuration options when constructing a new AudioContext object to represent a graph of web audio nodes.")

The  `**AudioContextOptions**`  dictionary is used to provide options when instantiating a new  `AudioContext`.

[`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")

The  **`AudioNode`**  interface represents an audio-processing module like an  _audio source_  (e.g. an HTML  [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio "The HTML <audio> element is used to embed sound content in documents. It may contain one or more audio sources, represented using the src attribute or the <source> element: the browser will choose the most suitable one. It can also be the destination for streamed media, using a MediaStream.")  or  [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "The HTML Video element (<video>) embeds a media player which supports video playback into the document. You can use <video> for audio content as well, but the <audio> element may provide a more appropriate user experience.")  element),  _audio destination_,  _intermediate processing module_  (e.g. a filter like  [`BiquadFilterNode`](https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode "The BiquadFilterNode interface represents a simple low-order filter, and is created using the AudioContext.createBiquadFilter() method. It is an AudioNode that can represent different kinds of filters, tone control devices, and graphic equalizers."), or  _volume control_  like  [`GainNode`](https://developer.mozilla.org/en-US/docs/Web/API/GainNode "The GainNode interface represents a change in volume. It is an AudioNode audio-processing module that causes a given gain to be applied to the input data before its propagation to the output. A GainNode always has exactly one input and one output, both with the same number of channels.")).

[`AudioParam`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam "The Web Audio API's AudioParam interface represents an audio-related parameter, usually a parameter of an AudioNode (such as GainNode.gain).")

The  **`AudioParam`**  interface represents an audio-related parameter, like one of an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:"). It can be set to a specific value or a change in value, and can be scheduled to happen at a specific time and following a specific pattern.

[`AudioParamMap`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParamMap "The Web Audio API interface AudioParamMap represents a set of multiple audio parameters, each described as a mapping of a DOMString identifying the parameter to the AudioParam object representing its value.")

Provides a maplike interface to a group of [`AudioParam`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam "The Web Audio API's AudioParam interface represents an audio-related parameter, usually a parameter of an AudioNode (such as GainNode.gain).")  interfaces, which means it provides the methods `forEach()`,  `get()`,  `has()`,  `keys()`, and `values()`, as well as a  `size`  property.

[`BaseAudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext "The BaseAudioContext interface of the Web Audio API acts as a base definition for online and offline audio-processing graphs, as represented by AudioContext and OfflineAudioContext respectively.")

The  **`BaseAudioContext`**  interface acts as a base definition for online and offline audio-processing graphs, as represented by  [`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext "The AudioContext interface represents an audio-processing graph built from audio modules linked together, each represented by an AudioNode.")  and  [`OfflineAudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/OfflineAudioContext "The OfflineAudioContext interface is an AudioContext interface representing an audio-processing graph built from linked together AudioNodes. In contrast with a standard AudioContext, an OfflineAudioContext doesn't render the audio to the device hardware; instead, it generates it, as fast as it can, and outputs the result to an AudioBuffer.")  respectively. You wouldn't use  `BaseAudioContext`  directly — you'd use its features via one of these two inheriting interfaces.

The  `[ended](https://developer.mozilla.org/en-US/docs/Web/Events/ended "/en-US/docs/Web/Events/ended")`  event

The  `ended`  event is fired when playback has stopped because the end of the media was reached.

### Defining audio sources

Interfaces that define audio sources for use in the Web Audio API.

[`AudioScheduledSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioScheduledSourceNode "The AudioScheduledSourceNode interface—part of the Web Audio API—is a parent interface for several types of audio source node interfaces which share the ability to be started and stopped, optionally at specified times. Specifically, this interface defines the start() and stop() methods, as well as the onended event handler.")

The  **`AudioScheduledSourceNode`**  is a parent interface for several types of audio source node interfaces. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:").

[`OscillatorNode`](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode "The OscillatorNode interface represents a periodic waveform, such as a sine wave. It is an AudioScheduledSourceNode audio-processing module that causes a specified frequency of a given wave to be created—in effect, a constant tone.")

The **`OscillatorNode`**  interface represents a periodic waveform, such as a sine or triangle wave. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  audio-processing module that causes a given _frequency_ of wave to be created.

[`AudioBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer "The AudioBuffer interface represents a short audio asset residing in memory, created from an audio file using the AudioContext.decodeAudioData() method, or from raw data using AudioContext.createBuffer(). Once put into an AudioBuffer, the audio can then be played by being passed into an AudioBufferSourceNode.")

The  **`AudioBuffer`**  interface represents a short audio asset residing in memory, created from an audio file using the  [`AudioContext.decodeAudioData()`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext/decodeAudioData "The documentation about this has not yet been written; please consider contributing!")  method, or created with raw data using  [`AudioContext.createBuffer()`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext/createBuffer "The documentation about this has not yet been written; please consider contributing!"). Once decoded into this form, the audio can then be put into an  [`AudioBufferSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBufferSourceNode "The AudioBufferSourceNode interface is an AudioScheduledSourceNode which represents an audio source consisting of in-memory audio data, stored in an AudioBuffer. It's especially useful for playing back audio which has particularly stringent timing accuracy requirements, such as for sounds that must match a specific rhythm and can be kept in memory rather than being played from disk or the network.").

[`AudioBufferSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBufferSourceNode "The AudioBufferSourceNode interface is an AudioScheduledSourceNode which represents an audio source consisting of in-memory audio data, stored in an AudioBuffer. It's especially useful for playing back audio which has particularly stringent timing accuracy requirements, such as for sounds that must match a specific rhythm and can be kept in memory rather than being played from disk or the network.")

The  **`AudioBufferSourceNode`**  interface represents an audio source consisting of in-memory audio data, stored in an  [`AudioBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer "The AudioBuffer interface represents a short audio asset residing in memory, created from an audio file using the AudioContext.decodeAudioData() method, or from raw data using AudioContext.createBuffer(). Once put into an AudioBuffer, the audio can then be played by being passed into an AudioBufferSourceNode."). It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that acts as an audio source.

[`MediaElementAudioSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/MediaElementAudioSourceNode "A MediaElementSourceNode has no inputs and exactly one output, and is created using the AudioContext.createMediaElementSource method. The amount of channels in the output equals the number of channels of the audio referenced by the HTMLMediaElement used in the creation of the node, or is 1 if the HTMLMediaElement has no audio.")

The  `**MediaElementAudioSourceNode**`  interface represents an audio source consisting of an HTML5  [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio "The HTML <audio> element is used to embed sound content in documents. It may contain one or more audio sources, represented using the src attribute or the <source> element: the browser will choose the most suitable one. It can also be the destination for streamed media, using a MediaStream.")  or  [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video "The HTML Video element (<video>) embeds a media player which supports video playback into the document. You can use <video> for audio content as well, but the <audio> element may provide a more appropriate user experience.")  element. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that acts as an audio source.

[`MediaStreamAudioSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamAudioSourceNode "The MediaStreamAudioSourceNode interface is a type of AudioNode which operates as an audio source whose media is received from a MediaStream obtained using the WebRTC or Media Capture and Streams APIs.")

The  `**MediaStreamAudioSourceNode**`  interface represents an audio source consisting of a  [`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream "The MediaStream interface represents a stream of media content. A stream consists of several tracks such as video or audio tracks. Each track is specified as an instance of MediaStreamTrack.")  (such as a webcam, microphone, or a stream being sent from a remote computer). If multiple audio tracks are present on the stream, the track whose  [`id`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack/id "The read-only property MediaStreamTrack.id returns a DOMString containing a unique identifier (GUID) for the track; it is generated by the browser.")  comes first lexicographically (alphabetically) is used. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that acts as an audio source.

[`MediaStreamTrackAudioSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrackAudioSourceNode "The MediaStreamTrackAudioSourceNode interface is a type of AudioNode which represents a source of audio data taken from a specific MediaStreamTrack obtained through the WebRTC or Media Capture and Streams APIs.")

A node of type  [`MediaStreamTrackAudioSourceNode`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrackAudioSourceNode "The MediaStreamTrackAudioSourceNode interface is a type of AudioNode which represents a source of audio data taken from a specific MediaStreamTrack obtained through the WebRTC or Media Capture and Streams APIs.")  represents an audio source whose data comes from a  [`MediaStreamTrack`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack "The MediaStreamTrack interface represents a single media track within a stream; typically, these are audio or video tracks, but other track types may exist as well."). When creating the node using the  [`createMediaStreamTrackSource()`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext/createMediaStreamTrackSource "A MediaStreamTrackAudioSourceNode object which acts as a source for audio data found in the specified audio track.")  method to create the node, you specify which track to use. This provides more control than  `MediaStreamAudioSourceNode`.

### Defining audio effects filters

Interfaces for defining effects that you want to apply to your audio sources.

[`BiquadFilterNode`](https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode "The BiquadFilterNode interface represents a simple low-order filter, and is created using the AudioContext.createBiquadFilter() method. It is an AudioNode that can represent different kinds of filters, tone control devices, and graphic equalizers.")

The  **`BiquadFilterNode`**  interface represents a simple low-order filter. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that can represent different kinds of filters, tone control devices, or graphic equalizers. A  `BiquadFilterNode`  always has exactly one input and one output.

[`ConvolverNode`](https://developer.mozilla.org/en-US/docs/Web/API/ConvolverNode "The ConvolverNode interface is an AudioNode that performs a Linear Convolution on a given AudioBuffer, often used to achieve a reverb effect. A ConvolverNode always has exactly one input and one output.")

The  `**Convolver**`**`Node`**  interface is an [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that performs a Linear Convolution on a given  [`AudioBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer "The AudioBuffer interface represents a short audio asset residing in memory, created from an audio file using the AudioContext.decodeAudioData() method, or from raw data using AudioContext.createBuffer(). Once put into an AudioBuffer, the audio can then be played by being passed into an AudioBufferSourceNode."), and is often used to achieve a reverb effect.

[`DelayNode`](https://developer.mozilla.org/en-US/docs/Web/API/DelayNode "The DelayNode interface represents a delay-line; an AudioNode audio-processing module that causes a delay between the arrival of an input data and its propagation to the output.")

The  **`DelayNode`**  interface represents a  [delay-line](http://en.wikipedia.org/wiki/Digital_delay_line "http://en.wikipedia.org/wiki/Digital_delay_line"); an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  audio-processing module that causes a delay between the arrival of an input data and its propagation to the output.

[`DynamicsCompressorNode`](https://developer.mozilla.org/en-US/docs/Web/API/DynamicsCompressorNode "Inherits properties from its parent, AudioNode.")

The  **`DynamicsCompressorNode`**  interface provides a compression effect, which lowers the volume of the loudest parts of the signal in order to help prevent clipping and distortion that can occur when multiple sounds are played and multiplexed together at once.

[`GainNode`](https://developer.mozilla.org/en-US/docs/Web/API/GainNode "The GainNode interface represents a change in volume. It is an AudioNode audio-processing module that causes a given gain to be applied to the input data before its propagation to the output. A GainNode always has exactly one input and one output, both with the same number of channels.")

The  **`GainNode`**  interface represents a change in volume. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  audio-processing module that causes a given  _gain_  to be applied to the input data before its propagation to the output.

[`WaveShaperNode`](https://developer.mozilla.org/en-US/docs/Web/API/WaveShaperNode "A WaveShaperNode always has exactly one input and one output.")

The  **`WaveShaperNode`**  interface represents a non-linear distorter. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that use a curve to apply a waveshaping distortion to the signal. Beside obvious distortion effects, it is often used to add a warm feeling to the signal.

[`PeriodicWave`](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicWave "PeriodicWave has no inputs or outputs; it is used to define custom oscillators when calling OscillatorNode.setPeriodicWave(). The PeriodicWave itself is created/returned by AudioContext.createPeriodicWave().")

Describes a periodic waveform that can be used to shape the output of an  [`OscillatorNode`](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode "The OscillatorNode interface represents a periodic waveform, such as a sine wave. It is an AudioScheduledSourceNode audio-processing module that causes a specified frequency of a given wave to be created—in effect, a constant tone.").

[`IIRFilterNode`](https://developer.mozilla.org/en-US/docs/Web/API/IIRFilterNode "The IIRFilterNode interface of the Web Audio API is a AudioNode processor which implements a general infinite impulse response (IIR)  filter; this type of filter can be used to implement tone control devices and graphic equalizers as well. It lets the parameters of the filter response be specified, so that it can be tuned as needed.")

Implements a general  **[infinite impulse response](https://en.wikipedia.org/wiki/infinite%20impulse%20response "infinite impulse response")**  (IIR) filter; this type of filter can be used to implement tone control devices and graphic equalizers as well.

### Defining audio destinations

Once you are done processing your audio, these interfaces define where to output it.

[`AudioDestinationNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioDestinationNode "AudioDestinationNode has no output (as it is the output, no more AudioNode can be linked after it in the audio graph) and one input. The number of channels in the input must be between 0 and the maxChannelCount value or an exception is raised.")

The  **`AudioDestinationNode`**  interface represents the end destination of an audio source in a given context — usually the speakers of your device.

[`MediaStreamAudioDestinationNode`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamAudioDestinationNode "Inherits properties from its parent, AudioNode.")

The  `**MediaStreamAudio**`**`DestinationNode`**  interface represents an audio destination consisting of a  [WebRTC](https://developer.mozilla.org/en-US/docs/WebRTC "/en-US/docs/WebRTC")  [`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream "The MediaStream interface represents a stream of media content. A stream consists of several tracks such as video or audio tracks. Each track is specified as an instance of MediaStreamTrack.")  with a single  `AudioMediaStreamTrack`, which can be used in a similar way to a  [`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream "The MediaStream interface represents a stream of media content. A stream consists of several tracks such as video or audio tracks. Each track is specified as an instance of MediaStreamTrack.")  obtained from  [`getUserMedia()`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia "The MediaDevices.getUserMedia() method prompts the user for permission to use a media input which produces a MediaStream with tracks containing the requested types of media."). It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that acts as an audio destination.

### Data analysis and visualization

If you want to extract time, frequency, and other data from your audio, the  `AnalyserNode`  is what you need.

[`AnalyserNode`](https://developer.mozilla.org/en-US/docs/Web/API/AnalyserNode "The AnalyserNode interface represents a node able to provide real-time frequency and time-domain analysis information. It is an AudioNode that passes the audio stream unchanged from the input to the output, but allows you to take the generated data, process it, and create audio visualizations.")

The  **`AnalyserNode`**  interface represents a node able to provide real-time frequency and time-domain analysis information, for the purposes of data analysis and visualization.

### Splitting and merging audio channels

To split and merge audio channels, you'll use these interfaces.

[`ChannelSplitterNode`](https://developer.mozilla.org/en-US/docs/Web/API/ChannelSplitterNode "The ChannelSplitterNode interface, often used in conjunction with its opposite, ChannelMergerNode, separates the different channels of an audio source into a set of mono outputs. This is useful for accessing each channel separately, e.g. for performing channel mixing where gain must be separately controlled on each channel.")

The  `**ChannelSplitterNode**`  interface separates the different channels of an audio source out into a set of  _mono_  outputs.

[`ChannelMergerNode`](https://developer.mozilla.org/en-US/docs/Web/API/ChannelMergerNode "The ChannelMergerNode interface, often used in conjunction with its opposite, ChannelSplitterNode, reunites different mono inputs into a single output. Each input is used to fill a channel of the output. This is useful for accessing each channels separately, e.g. for performing channel mixing where gain must be separately controlled on each channel.")

The  `**ChannelMergerNode**`  interface reunites different mono inputs into a single output. Each input will be used to fill a channel of the output.

### Audio spatialization

These interfaces allow you to add audio spatialization panning effects to your audio sources.

[`AudioListener`](https://developer.mozilla.org/en-US/docs/Web/API/AudioListener "The AudioListener interface represents the position and orientation of the unique person listening to the audio scene, and is used in audio spatialization. All PannerNodes spatialize in relation to the AudioListener stored in the BaseAudioContext.listener attribute.")

The  **`AudioListener`**  interface represents the position and orientation of the unique person listening to the audio scene used in audio spatialization.

[`PannerNode`](https://developer.mozilla.org/en-US/docs/Web/API/PannerNode "A PannerNode always has exactly one input and one output: the input can be mono or stereo but the output is always stereo (2 channels); you can't have panning effects without at least two audio channels!")

The  `**PannerNode**`  interface represents the position and behavior of an audio source signal in 3D space, allowing you to create complex panning effects.

[`StereoPannerNode`](https://developer.mozilla.org/en-US/docs/Web/API/StereoPannerNode "The pan property takes a unitless value between -1 (full left pan) and 1 (full right pan). This interface was introduced as a much simpler way to apply a simple panning effect than having to use a full PannerNode.")

The  `**StereoPannerNode**`  interface represents a simple stereo panner node that can be used to pan an audio stream left or right.

### Audio processing in JavaScript

Using audio worklets, you can define custom audio nodes written in JavaScript or  [WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly). Audio worklets implement the  [`Worklet`](https://developer.mozilla.org/en-US/docs/Web/API/Worklet "The Worklet interface is a lightweight version of Web Workers and gives developers access to low-level parts of the rendering pipeline.")  interface, a lightweight version of the  [`Worker`](https://developer.mozilla.org/en-US/docs/Web/API/Worker "The Worker interface of the Web Workers API represents a background task that can be easily created and can send messages back to its creator. Creating a worker is as simple as calling the Worker() constructor and specifying a script to be run in the worker thread.")  interface. Audio worklets are enabled by default for Chrome 66 or later.

[`AudioWorklet`](https://developer.mozilla.org/en-US/docs/Web/API/AudioWorklet "The AudioWorklet interface of the Web Audio API is used to supply custom audio processing scripts. User-supplied code is run in the AudioWorkletGlobalScope global execution context in a separate Web Audio rendering thread along with other nodes, allowing for zero-latency audio processing.")

The  `AudioWorklet`  interface is available via  [`BaseAudioContext.audioWorklet`](https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext/audioWorklet "The audioWorklet read-only property of the BaseAudioContext interface returns an instance of AudioWorklet that can be used for adding AudioWorkletProcessor-derived classes which implement custom audio processing."), and allows you to add new modules to the audio worklet.

[`AudioWorkletNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioWorkletNode "The AudioWorkletNode interface of the Web Audio API represents a base class for a user-defined AudioNode, which can be connected to an audio routing graph along with other nodes. It has an associated AudioWorkletProcessor, which does the actual audio processing in a Web Audio rendering thread.")

The  `AudioWorkletNode`  interface represents an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  that is embedded into an audio graph and can pass messages to the corresponding  `AudioWorkletProcessor`.

[`AudioWorkletProcessor`](https://developer.mozilla.org/en-US/docs/Web/API/AudioWorkletProcessor "The AudioWorkletProcessor interface of the Web Audio API represents an audio processing code behind a custom AudioWorkletNode. It lives in the AudioWorkletGlobalScope and runs on the Web Audio rendering thread. In turn, an AudioWorkletNode based on it runs on the main thread.")

The  `AudioWorkletProcessor`  interface represents audio processing code running in a  `AudioWorkletGlobalScope`  that generates, processes, or analyses audio directly, and can pass messages to the corresponding  `AudioWorkletNode`.

[`AudioWorkletGlobalScope`](https://developer.mozilla.org/en-US/docs/Web/API/AudioWorkletGlobalScope "The AudioWorkletGlobalScope interface of the Web Audio API represents a global execution context for user-supplied code, which defines custom AudioWorkletProcessor-derived classes. Each BaseAudioContext has a single AudioWorklet available under the audioWorklet property, which runs its code in a single AudioWorkletGlobalScope.")

The  `AudioWorkletGlobalScope`  interface is a  `WorkletGlobalScope`-derived object representing a worker context in which an audio processing script is run; it is designed to enable the generation, processing, and analysis of audio data directly using JavaScript in a worklet thread.

#### Obsolete: script processor nodes

Before audio worklets were defined, the Web Audio API used the  `ScriptProcessorNode` for JavaScript-based audio processing. Because the code runs in the main thread, they have bad performance. The  `ScriptProcessorNode`  is kept for historic reasons but is marked as deprecated and will be removed in a future version of the specification.

[`ScriptProcessorNode`](https://developer.mozilla.org/en-US/docs/Web/API/ScriptProcessorNode "The ScriptProcessorNode interface allows the generation, processing, or analyzing of audio using JavaScript.")

The  **`ScriptProcessorNode`**  interface allows the generation, processing, or analyzing of audio using JavaScript. It is an  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")  audio-processing module that is linked to two buffers, one containing the current input, one containing the output. An event, implementing the  [`AudioProcessingEvent`](https://developer.mozilla.org/en-US/docs/Web/API/AudioProcessingEvent)The Web Audio API AudioProcessingEvent represents events that occur when a ScriptProcessorNode input buffer is ready to be processed.")  interface, is sent to the object each time the input buffer contains new data, and the event handler terminates when it has filled the output buffer with data.

[`audioprocess`](https://developer.mozilla.org/en-US/docs/Web/Events/audioprocess)   (event)

The  `audioprocess`  event is fired when an input buffer of a Web Audio API  [`ScriptProcessorNode`](https://developer.mozilla.org/en-US/docs/Web/API/ScriptProcessorNode "The ScriptProcessorNode interface allows the generation, processing, or analyzing of audio using JavaScript.")  is ready to be processed.

[`AudioProcessingEvent`](https://developer.mozilla.org/en-US/docs/Web/API/AudioProcessingEvent "The Web Audio API AudioProcessingEvent represents events that occur when a ScriptProcessorNode input buffer is ready to be processed.")

The  [Web Audio API](https://developer.mozilla.org/en-US/docs/Web_Audio_API "/en-US/docs/Web_Audio_API")  `AudioProcessingEvent`  represents events that occur when a  [`ScriptProcessorNode`](https://developer.mozilla.org/en-US/docs/Web/API/ScriptProcessorNode "The ScriptProcessorNode interface allows the generation, processing, or analyzing of audio using JavaScript.")  input buffer is ready to be processed.

### Offline/background audio processing

It is possible to process/render an audio graph very quickly in the background — rendering it to an  [`AudioBuffer`](https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer "The AudioBuffer interface represents a short audio asset residing in memory, created from an audio file using the AudioContext.decodeAudioData() method, or from raw data using AudioContext.createBuffer(). Once put into an AudioBuffer, the audio can then be played by being passed into an AudioBufferSourceNode.")  rather than to the device's speakers — with the following.

[`OfflineAudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/OfflineAudioContext "The OfflineAudioContext interface is an AudioContext interface representing an audio-processing graph built from linked together AudioNodes. In contrast with a standard AudioContext, an OfflineAudioContext doesn't render the audio to the device hardware; instead, it generates it, as fast as it can, and outputs the result to an AudioBuffer.")

The  **`OfflineAudioContext`**  interface is an  [`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext "The AudioContext interface represents an audio-processing graph built from audio modules linked together, each represented by an AudioNode.")  interface representing an audio-processing graph built from linked together  [`AudioNode`](https://developer.mozilla.org/en-US/docs/Web/API/AudioNode "The AudioNode interface is a generic interface for representing an audio processing module. Examples include:")s. In contrast with a standard  `AudioContext`, an  `OfflineAudioContext`  doesn't really render the audio but rather generates it,  _as fast as it can_, in a buffer.

[`complete`](https://developer.mozilla.org/en-US/docs/Web/Events/complete) (event)

The  `complete`  event is fired when the rendering of an  [`OfflineAudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/OfflineAudioContext "The OfflineAudioContext interface is an AudioContext interface representing an audio-processing graph built from linked together AudioNodes. In contrast with a standard AudioContext, an OfflineAudioContext doesn't render the audio to the device hardware; instead, it generates it, as fast as it can, and outputs the result to an AudioBuffer.")  is terminated.

[`OfflineAudioCompletionEvent`](https://developer.mozilla.org/en-US/docs/Web/API/OfflineAudioCompletionEvent "The Web Audio API OfflineAudioCompletionEvent interface represents events that occur when the processing of an OfflineAudioContext is terminated. The complete event implements this interface.")

The  `OfflineAudioCompletionEvent`  represents events that occur when the processing of an  [`OfflineAudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/OfflineAudioContext "The OfflineAudioContext interface is an AudioContext interface representing an audio-processing graph built from linked together AudioNodes. In contrast with a standard AudioContext, an OfflineAudioContext doesn't render the audio to the device hardware; instead, it generates it, as fast as it can, and outputs the result to an AudioBuffer.")  is terminated. The  `[complete](https://developer.mozilla.org/en-US/docs/Web/Events/complete "/en-US/docs/Web/Events/complete")`  event implements this interface.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODU5NjE2MzE0XX0=
-->