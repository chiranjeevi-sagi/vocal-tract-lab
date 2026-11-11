# Vocal Tract Lab

An interactive 3D view of how the human voice is produced, driven by real speech analysis of any recording. Open or record up to 20 seconds of audio and watch the vocal folds, tongue, velum and nasal cavity move in step with the waveform, spectrogram and spectrum.

**Live page:** https://chiranjeevi-sagi.github.io/vocal-tract-lab/

Everything is one HTML file. There is no server, no build step and no external library. Audio is analysed in your browser and never leaves your computer.

## What you can do

- **Open or record audio.** Drag in a WAV, MP3, M4A, OGG or FLAC file, or press Record (R) to use the microphone. Longer files are cut to the first 20 seconds.
- **Explore the 3D head.** Drag to orbit, scroll to zoom, shift-drag to pan. Switch between X-ray, sagittal cut and tract-only views, and Head, Profile and Larynx cameras.
- **Scrub the timeline.** Click or drag on the waveform and spectrogram to move through the sound, scroll to zoom, and play at 1×, ½×, ¼× or ⅒× speed.
- **Read the scopes.** Vocal folds seen from above, the spectrum with the vocal tract filter and pitch harmonics overlaid, the tube area function, and an F1–F2 vowel chart.
- **Switch themes.** Dark and light.

Keyboard: Space plays and pauses, R records, the left and right arrows step 10 ms.

## How it works

Speech is modelled as a source passed through a filter. The vocal folds chop the airflow into pulses (the source) and the tube from the folds to the lips reshapes that buzz (the filter, a slowly varying linear system).

| Step | Method |
|---|---|
| Resampling | Windowed-sinc low-pass, then 16 kHz |
| Framing | 25 ms Hamming window every 5 ms, pre-emphasis 0.97 |
| Pitch and voicing | Normalised cross-correlation, 60–500 Hz, every 10 ms |
| Tract filter | 16-pole linear prediction (autocorrelation method, Levinson–Durbin) |
| Formants F1–F3 | Peaks of the LPC envelope |
| Tract shape | Reflection coefficients converted to a 16-section lossless tube (Wakita's inverse-filter method), 1.09 cm per section |
| Nasal sounds | A small neural network on 14 band energies with 20–80 ms of context |
| 3D rendering | Hand-written WebGL, no 3D library |

The built-in demo is synthesised with a Kelly–Lochbaum tube model, including a nasal side branch, so the analysis can be checked against known shapes.

## What is measured and what is modelled

Please read this before using the page for teaching or research.

- **Measured from the audio:** pitch, voicing, the spectrum, the LPC envelope and the formants.
- **Estimated:** the tract shape and therefore the tongue. Recovering a shape from sound alone has many solutions. The estimate is good for clear vowels and for the synthetic demo, and it is unreliable for fricatives, stops, noisy or echoing recordings.
- **Modelled, not measured:** the vocal fold motion. A microphone cannot see the folds. They are animated with a two-margin model at the measured pitch and loudness, slowed like a stroboscope.
- **The tongue** shows height and front-back position. It cannot show grooving, side contact or the tip curling.

### Nasal detection accuracy

The detector was fitted on 160 labelled American English sentences from 16 speakers (the TIMIT sample distributed with NLTK), with added noise, filtering, echo and speed changes. Figures below are for speakers it did not hear during fitting.

| Recording condition | Nasals of 50 ms or longer found | Very short nasals (20–45 ms) found |
|---|---|---|
| Clean | 92% | 66% |
| Phone-band or bass-cut microphone | 88–91% | 62–67% |
| Light white noise | 91% | 53% |
| Heavier low-frequency noise | 85% | 52% |
| Echo or a second voice in the background | 78% | 49–52% |

- About 1 flagged stretch in 6 on clean speech is not a nasal. These are usually the hum inside b, d or g, or a w or v.
- Whether the mouth is closed at the lips (m), the tongue tip (n) or the tongue back (ng) is right about 7 times in 10. The page draws its best guess and shows how sure it is.
- Other languages and accents have not been tested.

### Not suitable for

- Music with instruments. The analysis assumes one voice.
- High singing. Pitch tracking stops near 500 Hz.
- Clinical or diagnostic use of any kind.

## Privacy

- The page makes no network requests. A content security policy blocks them.
- It uses no cookies, no analytics and no browser storage.
- Recordings and uploaded files stay in memory in your browser tab and are gone when you close or reload it.

## Browser support

A current version of Chrome, Edge, Firefox or Safari with WebGL. Recording needs microphone permission and a page served over HTTPS (GitHub Pages is) or opened as a local file.

## Run it locally

Download `index.html` and open it in a browser. Nothing else is needed.

## Data and credits

- Nasal detector weights were fitted on the TIMIT sample distributed with NLTK. No audio or transcripts from that corpus are included here, only the fitted weights. 
- Method references: H. Wakita, "Direct estimation of the vocal tract shape by inverse filtering of acoustic speech waveforms" (1973); J. L. Kelly and C. C. Lochbaum, "Speech synthesis" (1962); G. Fant, *Acoustic Theory of Speech Production* (1960).
- Built with the help of Claude (Anthropic).

## License

This project is licensed under the MIT License.

## Author

Chiranjeevi Sagi
