# Sift
**Sift** is a lightweight, single-file HTML application designed for fast, local photo culling directly in your web browser. It automatically analyzes folders of JPEGs to flag blurry shots, closed eyes, exposure issues, and near duplicates, allowing you to quickly rate, filter, and organize photos without uploading your files anywhere.
### Features
* **Smart Auto-Analysis**
  * **Blur Detection:** Calculates image sharpness using Laplacian variance across focus tiles.
  * **Closed Eye Check:** Detects facial landmarks and eye aspect ratios using [MediaPipe Vision](https://github.com/google-ai-edge/mediapipe).
  * **Exposure Flags:** Highlights photos that are severely underexposed or overexposed.
  * **Burst & Duplicate Grouping:** Uses perceptual hashing (dHash) and EXIF capture timestamps to group bursts and recommend the best shot.
* **Local & Private:** All image decoding, analysis, and rendering happen locally in your browser using standard Web APIs. No image data is ever uploaded to a remote server.
* **Non-Destructive:** Rejects can be moved into a ```_Rejected``` subfolder inside your photo directory using the File System Access API—nothing is permanently deleted.
* **Keyboard-First Workflow:** Full keyboard nevigation for fast rating and status tagging.
* **Export Options:** Save your decisions, star ratings, color tags, and flag reasons to a structured CSV file (```culling-results.csv```).
## Requirements
* **Browser:** Google Chrome or Microsoft Edge on Desktop.
* **Image Format:** JPEG/JPG files.
* **Network Connection:** Internet access is needed on initial launch to load the MediaPipe face detection WebAssembly model.
## Quick Start
1. Download or clone ```sift.html``` to your local drive.
2. Open ```sift.html``` in Chrome or Edge.
3. Click **Open Folder** and choose a directory containing JPEG photos.
4. Allow directory permissions whem prompted by your browser.
5. Sift will analyze the folder and flag potential issues. Use the top filters (All, Undecided, Flagged, Picks, Rejects) to navigate.
6. When finished:
  * Click **Move rejects** to move rejected photos into a ```_Rejected``` subfolder.
  * Click **Export list** to save a CSV manifest of all ratings and tags.
## Keyboard Shortcuts
| Shortcut | Action |
| :--- | :--- |
| `→` / `↓` | Next photo |
| `←` / `↑` | Previous photo |
| `P` | Mark as **Pick** and advance |
| `X` | Mark as **Reject** and advance |
| `U` | Clear Pick/Reject status (Undecided) |
| `1` – `5` | Assign star rating (1 to 5 stars) |
| `0` | Clear star rating |
| `6` | Toggle **Red** label |
| `7` | Toggle **Yellow** label |
| `8` | Toggle **Green** label |
| `9` | Toggle **Blue** label |
| `Z` or `Space` | Toggle 100% zoom / full resolution view |
## Technical Details
* **File System Access API:** Uses ```window.showDirectoryPicker()``` to read files locally and create the ```_Rejected``` directory.
* **Image Processing:** Uses ```OffscreenCanvas``` for background thumbnail generatio, luma analysis, dHash generation, and Laplacian variance calculations.
* **EXIF Parsing:** Reads raw TIFF header blocks in the first 64KB of JPEG files to extract original ```DateTimeOriginal``` timestamps.
* **Face Detection:** Integrates ```@mediapipe/tasks-vision``` (```FaceLandmarker```) to measure eye blink blendshape values.
