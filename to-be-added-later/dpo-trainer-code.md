# DPO Trainer - Code

* &#x20;Will train or finetune our model using PEFT(SFT) then will retrain for prefrence alignment for controlled response using DPO

```
# Constructor: Initializes the dataset and configuration parameters.

# Prepares the LoRA model configuration for fine-tuning.
# Ensures 'self.model' is defined before calling 'get_peft_model' to avoid errors.

# Loads the pre-trained model with 4-bit precision for memory efficiency.
# Maps the model to the correct device (CPU/GPU).
# Disables cache to allow gradient updates during training.
# Prepares the model for low-bit fine-tuning using LoRA if enabled in the 
# configuration.

# Loads the tokenizer and sets the pad token.
# Setting pad_token as eos_token is a common practice, but check if it aligns with
# your dataset needs.

# Defines training arguments such as batch size, optimizer, learning rate, and 
# number of training epochs.
# Uses 'fp16' for mixed precision training to optimize performance.
# Disables logging to external platforms using 'report_to="none"'.

# Creates the trainer after loading the model and tokenizer.
# Prints trainable parameters when using LoRA for debugging.
# Initializes the trainer with model, dataset, and training arguments.

# Starts the training process.
# Saves the fine-tuned model and tokenizer in the specified output directory.
# Ensures the tokenizer is saved properly for later inference.
```

```python
!pip install transformers trl peft accelerate datasets bitsandbytes

from huggingface_hub import notebook_login
notebook_login()

from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig, TrainingArguments
from datasets import load_dataset, Dataset
from trl import SFTTrainer, DPOConfig, DPOTrainer
from peft import LoraConfig, AutoPeftModelForCausalLM, prepare_model_for_kbit_training, get_peft_model
import torch
from accelerate import Accelerator


## parameter for the SFT
sft_config={
    "model_ckpt": "TinyLlama/TinyLlama-1.1B-Chat-v1.0", #This is my Pretrain model
            "load_in_4bit": True,
            "device_map": {"": Accelerator().local_process_index},
            "torch_dtype": torch.float16,
            "torch_dtype": torch.float16,
            "trust_remote_code": True,

            "use_lora": True,
            "r": 16,
            "lora_alpha": 16,
            "lora_dropout": 0.05,
            "bias": "none",
            "task_type": "CAUSAL_LM",
            "target_modules": ["q_proj", "v_proj"],

            "output_dir": "sft-tiny-chatbot",
            "per_device_train_batch_size": 1,
            "gradient_accumulation_steps": 1,
            "optim": "paged_adamw_32bit",
            "learning_rate": 2e-4,
            "lr_scheduler_type": "cosine",
            "save_strategy": "epoch",
            "logging_steps": 100,
            "num_train_epochs": 1,
            "max_steps": 250,
            "fp16": True,
            "push_to_hub": True,
            "packing": False,
            "max_seq_length": 512,
            "neftune_noise_alpha": 5
}

class TrainSFT:
  def __init__(self,data,config) -> None:
    self.data=data
    self.config=config

  def prepare_lora_model(self):
    self.lora_config=LoraConfig(r=self.config["r"],
                                lora_alpha=self.config["lora_alpha"],
                                lora_dropout=self.config["lora_dropout"],
                                bias=self.config["bias"],
                                task_type=self.config["task_type"],
                                target_modules=self.config["target_modules"])

    self.model=get_peft_model(self.model,self.lora_config)

  def load_model_tokenizer(self):
    self.model=AutoModelForCausalLM.from_pretrained(
                              self.config["model_ckpt"],
                              load_in_4bit=self.config["load_in_4bit"],
                              device_map=self.config["device_map"],
                              torch_dtype=self.config["torch_dtype"]
      )
    self.model.config.use_cache=False
    self.model.config.pretraining_tp=1
    self.model = prepare_model_for_kbit_training(self.model)

    if self.config["use_lora"]:
      self.prepare_lora_model()

    self.tokenizer = AutoTokenizer.from_pretrained(self.config["model_ckpt"])
    self.tokenizer.pad_token = self.tokenizer.eos_token

  def set_training_args(self):
      return TrainingArguments(
                              output_dir=self.config["output_dir"],
                              per_device_train_batch_size=self.config["per_device_train_batch_size"],
                              gradient_accumulation_steps=self.config["gradient_accumulation_steps"],
                              optim=self.config["optim"],
                              learning_rate=self.config["learning_rate"],
                              lr_scheduler_type=self.config["lr_scheduler_type"],
                              save_strategy=self.config["save_strategy"],
                              logging_steps=self.config["logging_steps"],
                              num_train_epochs=self.config["num_train_epochs"],
                              max_steps=self.config["max_steps"],
                              fp16=self.config["fp16"],
                              push_to_hub=self.config["push_to_hub"],
                              neftune_noise_alpha=self.config["neftune_noise_alpha"],
                              report_to="none"
                                )

  def create_trainer(self):
    self.load_model_tokenizer()
    if self.config["use_lora"]:
            print(self.model.print_trainable_parameters())
            self.trainer = SFTTrainer(
                              model=self.model,
                              train_dataset=self.data,
                              peft_config=self.lora_config,
                              args=self.set_training_args(),
                              tokenizer=self.tokenizer,
                                )


  def train_and_save_model(self):
    self.create_trainer()
    self.trainer.train()
    self.trainer.save_model(self.config["output_dir"])
    self.tokenizer.save_pretrained(self.config["output_dir"])

def create_data():
  data=load_dataset("tatsu-lab/alpaca", split="train")
  data_df = data.to_pandas()
  data_df = data_df[:700]
  data_df["text"] = data_df[["input", "instruction", "output"]].apply(lambda x: "Human: " + x["instruction"] + " " + x["input"] + " Assistant: "+ x["output"], axis=1)
  data = Dataset.from_pandas(data_df)
  return data

data=create_data()

data[0]
# {'instruction': 'Give three tips for staying healthy.',
#  'input': '',
#  'output': '1.Eat a balanced diet and make sure to include plenty of fruits and vegetables. \n2. Exercise regularly to keep your body active and strong. \n3. Get enough sleep and maintain a consistent sleep schedule.',
#  'text': 'Human: Give three tips for staying healthy.  Assistant: 1.Eat a balanced diet and make sure to include plenty of fruits and vegetables. \n2. Exercise regularly to keep your body active and strong. \n3. Get enough sleep and maintain a consistent sleep schedule.'}

train_sft=TrainSFT(data,sft_config)
train_sft.train_and_save_model()

dpo_config = {
            "model_ckpt": "snshrivas10/sft-tiny-chatbot",
            "load_in_4bit": True,
            "device_map": {"": Accelerator().local_process_index},
            "torch_dtype": torch.float16,
            "trust_remote_code": True,
            "use_lora": True,
            "r": 8,
            "lora_alpha": 8,
            "lora_dropout": 0.05,
            "bias": "none",
            "task_type": "CAUSAL_LM",
            "target_modules": ["q_proj", "v_proj"],
            "output_dir": "tiny-chatbot-dpo",
            "per_device_train_batch_size": 1,
            "gradient_accumulation_steps": 1,
            "optim": "paged_adamw_32bit",
            "learning_rate": 2e-4,
            "lr_scheduler_type": "cosine",
            "save_strategy": "epoch",
            "logging_steps": 100,
            "num_train_epochs": 1,
            "max_steps": 250,
            "fp16": True,
            "push_to_hub": True,
            "train_cln_name": "text",
            "packing": False,
            "neftune_noise_alpha": 5,
            "beta": 0.1,
            "loss_type": "kto_pair",
            "max_length": 768,
            "max_prompt_length": 512,
            "max_target_length": 256,
            "is_encoder_decoder": False
        }
        
class TrainDPO:
  def __init__(self,data,config) -> None:
    self.data=data
    self.config=config

  def prepare_lora_model(self):
    self.lora_config = LoraConfig(
                                r=self.config["r"],
                                lora_alpha=self.config["lora_alpha"],
                                lora_dropout=self.config["lora_dropout"],
                                bias=self.config["bias"],
                                task_type=self.config["task_type"],
                                target_modules=self.config["target_modules"]
                                )
    self.model = get_peft_model(self.model, self.lora_config) #THIS MODEL WHICH I AM GOING TO TRAIN FURTHER ON MY HUMAN PREFRENCE DATA
    self.model_ref = get_peft_model(self.model_ref, self.lora_config) #SAME MODEL IN THIS VARIABLE FOR THE REFRENCE #FROZAN WEIGHTS



  def load_model_tokenizer(self):
        self.model = AutoModelForCausalLM.from_pretrained(
                            self.config["model_ckpt"],
                            load_in_4bit=self.config["load_in_4bit"],
                            device_map=self.config["device_map"],
                            torch_dtype=self.config["torch_dtype"]
                        )

        self.model_ref = AutoModelForCausalLM.from_pretrained(
                            self.config["model_ckpt"],
                            load_in_4bit=self.config["load_in_4bit"],
                            device_map=self.config["device_map"],
                            torch_dtype=self.config["torch_dtype"]
                        )
        self.model.config.use_cache=False
        self.model.config.pretraining_tp=1
        self.model = prepare_model_for_kbit_training(self.model)
        if self.config["use_lora"]:
            self.prepare_lora_model()

        self.tokenizer = AutoTokenizer.from_pretrained(self.config["model_ckpt"])
        self.tokenizer.pad_token = self.tokenizer.eos_token

  def set_training_args(self):
    return DPOConfig(
                        output_dir=self.config["output_dir"],
                        per_device_train_batch_size=self.config["per_device_train_batch_size"],
                        gradient_accumulation_steps=self.config["gradient_accumulation_steps"],
                        optim=self.config["optim"],
                        learning_rate=self.config["learning_rate"],
                        lr_scheduler_type=self.config["lr_scheduler_type"],
                        save_strategy=self.config["save_strategy"],
                        logging_steps=self.config["logging_steps"],
                        num_train_epochs=self.config["num_train_epochs"],
                        max_steps=self.config["max_steps"],
                        fp16=self.config["fp16"],
                        push_to_hub=self.config["push_to_hub"],
                        neftune_noise_alpha=self.config["neftune_noise_alpha"],
                        report_to="none",
                        remove_unused_columns=False,

                            )

  def create_trainer(self):
    self.load_model_tokenizer()

    if self.config["use_lora"]:
        print(self.model.print_trainable_parameters())
        self.trainer = DPOTrainer(
                                  self.model,
                                  self.model_ref,
                                  args=self.set_training_args(),
                                  train_dataset=self.data,
                                  tokenizer=self.tokenizer,

                                )

  def train_and_save_model(self):
    self.create_trainer()
    self.trainer.train()
    self.trainer.save_model(self.config["output_dir"])
    self.tokenizer.save_pretrained(self.config["output_dir"])


def create_data():
    df = load_dataset("Anthropic/hh-rlhf", split="train").to_pandas()
    df["prompt"] = df["chosen"].apply(lambda x: x.split("Assistant: ")[0])
    df["chosen"] = df["chosen"].apply(lambda x: "Assistant: "+ x.split("Assistant: ")[-1])
    df["rejected"] = df["rejected"].apply(lambda x: "Assistant: " + x.split("Assistant: ")[-1])
    df = df.sample(1000)
    data = Dataset.from_pandas(df)
    return data

data=create_data()
data[0]
# {'chosen': 'Assistant: I’m sorry, I’m not sure what I could clarify here.  The Normandy Empire eventually became the Kingdom of England, which is what you see as England today.  That was in 1066.  Is there anything else I could clarify?',
#  'rejected': 'Assistant: Which question was that?  I’m not sure what else you might be asking, so I’m hoping you’ll let me know.',
#  'prompt': '\n\nHuman: At what point in history did Christianity replace all other religions in Western Europe?\n\n',
#  '__index_level_0__': 59219}

train_dpo=TrainDPO(data,dpo_config)
train_dpo.train_and_save_model()
```
