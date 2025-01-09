# Fine_Tuning_GPT2

## 1. Cleaning the Short Stories File
<img width="646" alt="Screenshot 2025-01-09 at 10 16 30 PM" src="https://github.com/user-attachments/assets/bb5e333e-d1b7-4e27-be72-f27f5dac982c" />


Information about the text file:

a) Have to clean promotions given before the EOS

b) Already has delimiters like SOS, LN, and EOS

Steps taken to clean the file 

c) Defined unwanted patterns and removed them :

unwanted_pattern = r'<nl>.*?(\[.*?\]\(http[s]?://\S+\)|subscribe|/r/|PART \d+).*?<eos>'

## 2. Dataset Loading and Tokenization

Installing Dependencies:

a) transformaters: Hugging Face library to load and fine-tune models like GPT-2.

b) torch: PyTorch library(backend for model traning).

c) accelerate: A utility to help scale training across multiple GPUs and hardware.

### 2.1 Initializing the GPT-2 Tokenizer
<img width="903" alt="Screenshot 2025-01-09 at 10 43 27 PM" src="https://github.com/user-attachments/assets/08a75a17-1faa-4b5a-b9f2-0c3219933534" />

a) Convert raw text into input IDs(token IDs) and also set labels for language modeling.

b) Use padding = "max_length" to force all sequences to length 512(with padding).

c) truncation = True ensures any text longer than 512 tokens is truncated.

## 3. Train / Validation Split and Model Initialization

<img width="717" alt="Screenshot 2025-01-09 at 11 07 15 PM" src="https://github.com/user-attachments/assets/b13e1479-7879-4c5a-855f-c4b1ae579759" />

a) Splits the dataset into 90% training data and 10% validation data for model evaluation

b) Loads GPT-2 with the language modeling head attached (GPT2LMHeadModel)

## 4. Fine-Tuning the Model
### 4.1 Defining Training Arguments
<img width="615" alt="Screenshot 2025-01-09 at 11 13 55 PM" src="https://github.com/user-attachments/assets/f0749d86-ef4a-4382-8141-a286af16b6f1" />

Key Parameters:

a) output_dir and logging_dir define where model checkpoints and logs are saved.

b) num_train_epochs = 3 runs the training loop for 3 epochs.

c) per_device_train_batch_size = 8 and per_device_eval_batch_size = 8 define batch sizes.

d) warmup_steps=500 sets a learning rate warmup period.

e) weight_decay=0.01 applies a small weight regularization to avoid overfitting.


### 4.2 

<img width="364" alt="Screenshot 2025-01-09 at 11 32 41 PM" src="https://github.com/user-attachments/assets/512389f7-88bc-4e3a-b221-36b1c23ce52a" />

Initializing the Trainer

Trainer is a high-level API from Hugging Face which handles training loops, evaluation, logging, checkpoints, etc.

### Training Logs

<img width="449" alt="Screenshot 2025-01-09 at 11 32 12 PM" src="https://github.com/user-attachments/assets/ec4a7b37-f05b-4afc-8aca-8e2a7a13fedd" />

The model is converging around a validation loss in the 2.28 - 2.29 range, which suggests it has learned some patterns, but there is still room for imporvement.

## 5. Generating Text with the Fine-Tuned Model

### 5.1 Loading the Fine-Tuned Model in a Pipeline

<img width="799" alt="Screenshot 2025-01-09 at 11 35 42 PM" src="https://github.com/user-attachments/assets/4ebe5bb9-face-4c29-8934-237353cf3cdf" />

Parameters:

a) model="./gpt2-fine-tuned": Points to the directory where the fine-tuned model is saved.

b) tokenizer=tokenizer: The GPT-2 tokenizer used during training.

c) generator(...): Feeds a prompt ("Once upon a time,") into the model and asks for up to max_length=100 tokens.

d) num_return_sequences=1 returns one generated text sample.

### 5.2 Examples of Text Generated
<img width="1051" alt="Screenshot 2025-01-09 at 11 28 53 PM" src="https://github.com/user-attachments/assets/222dbb6e-5f79-4c51-be40-8aa45803a58b" />

<img width="1043" alt="Screenshot 2025-01-09 at 11 29 16 PM" src="https://github.com/user-attachments/assets/48b7af25-e985-4a29-ae60-520200f30bd0" />

### 6. Conclusions

Validation losses around 2.28–2.29 indicate the model has learned the dataset but may not be fully optimized. Further tuning (longer training, different hyperparameters, or more data) could improve performance. The model outputs coherent text. Some randomness or tangential content is expected, as GPT-2 is a generative model.
Adjusting parameters like temperature, top_k, or top_p can shape output style and coherence.

### 7. Project Showcase

This fine-tuning work has been showcased on [Hugging Face Spaces](https://huggingface.co/spaces/a1zubair/fine_tune_gpt2_short_story)
allowing anyone to interact with and test the model directly.

