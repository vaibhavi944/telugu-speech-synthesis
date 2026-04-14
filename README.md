---
library_name: transformers
license: mit
base_model: microsoft/speecht5_tts
tags:
- generated_from_trainer
- text-to-speech
- tts
- telugu
- indian-languages
- speech-synthesis
- speecht5
- audio-generation
language:
- te
datasets:
- indictt5
model-index:
- name: speecht5_finetuned_telugu_charan
  results:
  - task:
      type: text-to-speech
      name: Text-to-Speech
    dataset:
      type: indictt5
      name: IndicTTS Telugu
    metrics:
    - type: loss
      value: 0.4496
      name: Validation Loss
---

# Telugu TTS — SpeechT5 Fine-tuned

A fine-tuned text-to-speech model built on top of [microsoft/speecht5_tts](https://huggingface.co/microsoft/speecht5_tts), adapted specifically for natural Telugu speech synthesis using the IndicTTS dataset.

## About

This model brings high-quality Telugu speech generation by fine-tuning Microsoft's SpeechT5 architecture on native Telugu audio data. Key focus areas during development:

- Telugu-specific phoneme handling and conversion
- Custom transliteration pipeline for Telugu script
- EDA-driven phoneme balancing to improve robustness
- Optimized token mappings for Telugu characters

## Intended Uses

- Telugu accessibility tools for visually impaired users
- Language learning and educational applications
- Voiceover generation for Telugu multimedia content
- Indian language speech synthesis research

## Limitations

- Optimized for Telugu only — other languages not supported
- May struggle with foreign words, technical terms, or code-mixed text
- Regional accent variations may not be fully captured

## Training Data

Trained on the **IndicTTS Telugu dataset** — **8,576 high-quality Telugu audio samples** with:

- Custom transliteration and phoneme conversion pipeline
- Phoneme distribution balancing via EDA
- Refined Telugu character to token mappings
- Quality-filtered audio-text pairs

## Training Configuration

| Parameter | Value |
|-----------|-------|
| Learning Rate | 0.001 |
| Batch Size | 4 |
| Gradient Accumulation | 8 steps (effective batch: 32) |
| Optimizer | AdamW (β=0.9, 0.999, ε=1e-08) |
| LR Scheduler | Linear + 100 warmup steps |
| Total Steps | 1,000 |
| Precision | Native AMP |

## Training Results

| Training Loss | Epoch  | Step | Validation Loss |
|:-------------:|:------:|:----:|:---------------:|
| 0.7785        | 0.4145 | 100  | 0.6689          |
| 0.8247        | 0.8290 | 200  | 0.7610          |
| 0.6961        | 1.2404 | 300  | 0.6406          |
| 0.6305        | 1.6549 | 400  | 0.5726          |
| 0.5784        | 2.0663 | 500  | 0.5422          |
| 0.5582        | 2.4808 | 600  | 0.5184          |
| 0.5399        | 2.8953 | 700  | 0.4992          |
| 0.5132        | 3.3067 | 800  | 0.4786          |
| 0.4903        | 3.7212 | 900  | 0.4617          |
| 0.4774        | 4.1326 | 1000 | 0.4496          |

Final validation loss: **0.4496**

## Usage

```python
from transformers import SpeechT5Processor, SpeechT5ForTextToSpeech
import torch

processor = SpeechT5Processor.from_pretrained("your-username/speecht5_finetuned_telugu_charan")
model = SpeechT5ForTextToSpeech.from_pretrained("your-username/speecht5_finetuned_telugu_charan")

text = "మీ Telugu వాక్యం ఇక్కడ రాయండి"
inputs = processor(text=text, return_tensors="pt")

with torch.no_grad():
    speech = model.generate_speech(inputs["input_ids"], speaker_embeddings=None)
```

## Framework Versions

- Transformers: 4.47.0
- PyTorch: 2.5.1+cu121
- Datasets: 3.3.1
- Tokenizers: 0.21.0

## Citation

```bibtex
@misc{speecht5_telugu_charan,
  title={SpeechT5 Fine-tuned for Telugu Text-to-Speech},
  author={Rama Charan Pisupati},
  year={2025},
  howpublished={\url{https://huggingface.co/your-username/speecht5_finetuned_telugu_charan}}
}
```

## Acknowledgments

- Microsoft Research for the SpeechT5 architecture
- IndicTTS team for the Telugu dataset
- Hugging Face for the transformers library and hosting
