# gpt2-text-generator-webapp

A lightweight Flask web application that serves a fine-tuned GPT-2 model for interactive text generation. Enter a prompt in the browser and get AI-generated continuations in real time.

---

## Features

- **Fine-tuned GPT-2** — loads a locally fine-tuned `GPT2LMHeadModel` from `./fine_tuned_gpt2`
- **Flask web interface** — simple single-page UI to submit prompts and display results
- **Configurable generation** — beam search (5 beams), top-p sampling (0.95), temperature (1.0), max 50 tokens
- **Error handling** — gracefully catches tokenization and generation errors and surfaces them in the UI

---

## Project Structure

```
.
├── app.py                  # Flask app — routes, model loading, text generation
├── config.json             # GPT-2 model architecture config (12-layer, 768-dim)
├── generation_config.json  # Generation defaults (BOS/EOS token IDs)
├── merges.txt              # BPE merge rules for the GPT-2 tokenizer
└── README.md
```

> **Note:** The fine-tuned model weights (`./fine_tuned_gpt2/`) are not included in this repository. See setup instructions below.

---

## Requirements

- Python 3.8+
- PyTorch
- Hugging Face Transformers
- Flask

Install dependencies:

```bash
pip install flask torch transformers
```

---

## Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/gpt2-text-generator-webapp.git
   cd gpt2-text-generator-webapp
   ```

2. **Add your fine-tuned model**

   Place your fine-tuned GPT-2 weights and tokenizer files in a folder named `fine_tuned_gpt2/` at the project root:
   ```
   fine_tuned_gpt2/
   ├── config.json
   ├── pytorch_model.bin   # or model.safetensors
   ├── tokenizer.json
   └── vocab.json
   ```

   If you don't have a fine-tuned model yet, the app will fall back to base GPT-2 if you point `model_path` to `'gpt2'` in `app.py`.

3. **Run the app**
   ```bash
   python app.py
   ```

4. Open your browser at `http://127.0.0.1:5000`

---

## Usage

1. Type a text prompt into the input box.
2. Click **Generate**.
3. The model completes your prompt and displays the result below.

Generation parameters (editable in `app.py`):

| Parameter | Value | Effect |
|---|---|---|
| `max_length` | 50 | Maximum tokens in output |
| `num_beams` | 5 | Beam search width |
| `top_p` | 0.95 | Nucleus sampling cutoff |
| `temperature` | 1.0 | Sampling randomness |

---

## Model Configuration

The included `config.json` describes the base GPT-2 architecture:

- **Layers:** 12 transformer blocks
- **Hidden size:** 768
- **Attention heads:** 12
- **Context window:** 1024 tokens
- **Vocabulary size:** 50,257
