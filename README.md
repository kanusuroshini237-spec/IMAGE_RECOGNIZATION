# 🖼️ Image Recognition App

A simple Streamlit web app that classifies images into 1,000 everyday object categories (ImageNet) using pretrained models from Hugging Face. Upload a photo or snap one with your webcam, and the model tells you what it sees.

**No API key required.** After the first model download, it runs fully offline.

## Features

- 📁 Upload images (JPG, JPEG, PNG, WEBP, BMP)
- 📷 Capture photos directly from your camera
- 🔄 Choose between two models:
  - **ViT Base** (`google/vit-base-patch16-224`)
  - **ResNet-50** (`microsoft/resnet-50`)
- 🎚️ Adjustable number of predictions (1 to 10)
- 📊 Confidence scores shown with progress bars

## Requirements

- Python 3.9 or newer
- ~1 GB free disk space (for PyTorch and model weights)
- Internet connection on first run only (to download the model)

## Installation

1. **Clone or download** this project and save the script as `app.py`.

2. **(Recommended) Create a virtual environment:**

   ```bash
   python -m venv venv
   source venv/bin/activate        # macOS / Linux
   venv\Scripts\activate           # Windows
   ```

3. **Install dependencies:**

   ```bash
   pip install streamlit pillow transformers torch
   ```

   Or save the following as `requirements.txt` and run `pip install -r requirements.txt`:

   ```text
   streamlit
   pillow
   transformers
   torch
   ```

## Usage

```bash
streamlit run app.py
```

The app opens in your browser at `http://localhost:8501`.

1. Pick a model and the number of predictions in the sidebar.
2. Upload an image, or switch to the **Camera** tab and take a picture.
3. Click **🔍 Recognize**.
4. View the top prediction and the full list of results with confidence scores.

## How It Works

The app uses the Hugging Face `pipeline("image-classification")` API. The selected model is loaded once and cached with `st.cache_resource`, so switching images does not reload it. Each image is converted to RGB, passed to the model, and the top-k labels with their probabilities are displayed.

## Notes

- **First run is slower.** Model weights download automatically (ViT Base is roughly 350 MB, ResNet-50 roughly 100 MB) and are stored in `~/.cache/huggingface/`. Later runs load from this cache.
- **Limited to ImageNet classes.** The models only recognize the 1,000 categories they were trained on (animals, vehicles, household objects, food, and so on). They cannot identify specific people, text, or things outside those classes, and will return the closest match instead.
- **Camera access.** Your browser will ask for permission to use the webcam. Camera input generally requires `localhost` or HTTPS.
- **CPU is fine.** A GPU is not required, though inference is faster with one.

## Troubleshooting

| Problem | Fix |
|---|---|
| `ModuleNotFoundError` | Make sure your virtual environment is active and dependencies are installed. |
| Model fails to download | Check your internet connection; the first run needs access to huggingface.co. |
| Camera tab shows nothing | Allow camera permission in your browser and make sure no other app is using the webcam. |
| Slow predictions | Try the ResNet-50 model, which is lighter than ViT Base. |

## Tech Stack

- [Streamlit](https://streamlit.io/): web UI
- [Hugging Face Transformers](https://huggingface.co/docs/transformers): model pipeline
- [PyTorch](https://pytorch.org/): inference backend
- [Pillow](https://python-pillow.org/): image handling

## License

Add your preferred license here (e.g. MIT). Note that the pretrained models have their own licenses on Hugging Face.
