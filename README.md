# _WildfireVLM_

This repository tracks the WildfireVLM project and now includes instructions to publish project artifacts on the Hugging Face Hub for better discoverability.

## Hugging Face paper page

If you are one of the paper authors, submit the paper at:

- https://huggingface.co/papers/submit

After submission, add links to:

- This GitHub repository
- Project page (if available)
- Model and dataset repos published below

## Publish WildfireVLM model checkpoints on Hugging Face

Create one Hub model repository per checkpoint (recommended), for example:

- `your-hf-org-or-username/WildfireVLM-base`
- `your-hf-org-or-username/WildfireVLM-large`

### Option A: Upload from local files

```bash
pip install -U "huggingface_hub[cli]"
huggingface-cli login

# Create each model repository once
huggingface-cli repo create your-hf-org-or-username/WildfireVLM-base --type model

# Upload checkpoint files
huggingface-cli upload your-hf-org-or-username/WildfireVLM-base ./checkpoints/base --repo-type model
```

### Option B: Integrate push/load in PyTorch code

```python
from huggingface_hub import PyTorchModelHubMixin
import torch.nn as nn

class WildfireVLM(nn.Module, PyTorchModelHubMixin):
    def __init__(self, ...):
        super().__init__()
        ...

model = WildfireVLM(...)
model.push_to_hub("your-hf-org-or-username/WildfireVLM-base")

# Later
reloaded = WildfireVLM.from_pretrained("your-hf-org-or-username/WildfireVLM-base")
```

## Publish WildfireVLM dataset on Hugging Face

Create and upload a dataset repository, for example:

- `your-hf-org-or-username/WildfireVLM-dataset`

```bash
huggingface-cli repo create your-hf-org-or-username/WildfireVLM-dataset --type dataset
huggingface-cli upload your-hf-org-or-username/WildfireVLM-dataset ./data --repo-type dataset
```

Users can then load it with:

```python
from datasets import load_dataset

dataset = load_dataset("your-hf-org-or-username/WildfireVLM-dataset")
```

## Helpful links

- Model upload guide: https://huggingface.co/docs/hub/models-uploading
- `PyTorchModelHubMixin`: https://huggingface.co/docs/huggingface_hub/package_reference/mixins#huggingface_hub.PyTorchModelHubMixin
- `hf_hub_download`: https://huggingface.co/docs/huggingface_hub/en/guides/download#download-a-single-file
- Dataset loading guide: https://huggingface.co/docs/datasets/loading
- Dataset viewer: https://huggingface.co/docs/hub/en/datasets-viewer
