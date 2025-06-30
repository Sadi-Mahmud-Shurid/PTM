## Acknowledgments

This repository is based on the work by [Abhijit Mishra](https://github.com/abhijitmishra) and collaborators in the [Thought2Text](https://github.com/abhijitmishra/Thought2Text) project.

Original Paper: [Thought2Text: Text Generation from EEG Signal using Large Language Models (LLMs)](https://arxiv.org/pdf/2410.07507v1)

Original Codebase: https://github.com/abhijitmishra/Thought2Text

Only minor changes and adaptations have been made in this version. All credit for the core implementation goes to the original authors.


## How to Reproduce:
1. First clone the above-mentioned repository in the working directory.
2. Download the data from the following link and store it in the same directory: https://drive.google.com/drive/folders/1XqV6MMl28iYXkQBMEFHfEXllGmCbqpOu
3. Run the following command to install all the dependencies: pip install -r requirements.txt 
(Note that the program might prompt you to install different versions than the one mentioned in the requirements.txt, follow the prompt in that case otherwise, the program runs into error in later stages)
4. Run the following command for EEG Encoder alignment with CLIP embeddings: 
python train_eeg_classifier.py --eeg_dataset data/block/eeg_55_95_std.pth --splits_path data/block/block_splits_by_image_all.pth --output ./eeg_encoder_55-95_40_classes --image_dir data/images/
5. Run the following fine-tuning command: 

python finetune_llm.py \
    --eeg_dataset data/block/eeg_55_95_std.pth \
    --splits_path data/block/block_splits_by_image_all.pth \
    --eeg_encoder_path ./eeg_encoder_55-95_40_classes \
    --image_dir data/images/ \
    --output "deepseek_eeg_model_7B_base" \
    --llm_backbone_name_or_path "deepseek-ai/deepseek-llm-7b-base" \
    --load_in_8bit \
    --bf16

6. For inference:

python inference.py \
    --model_path "deepseek_eeg_model_7B_base/" \
    --eeg_dataset data/block/eeg_55_95_std.pth \
    --image_dir data/images/ \
    --dest "deepseek_eeg_model_7B_base_results.csv" \
    --splits_path data/block/block_splits_by_image_all.pth
