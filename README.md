# TCM-BERT

The implementation of TCM-BERT in our paper:

Liang Yao, Zhe Jin, Chengsheng Mao, Yin Zhang and Yuan Luo. "Traditional Chinese Medicine Clinical Records Classification with BERT and Domain Specific Corpora." Journal of the American Medical Informatics Association (JAMIA). Volume 26, Issue 12, December 2019, Pages 1632–1636, https://doi.org/10.1093/jamia/ocz164

The repository is modified from [pytorch-pretrained-BERT](https://github.com/huggingface/pytorch-pretrained-BERT) and tested on Python 3.5+.

## Installing requirement packages

```bash
pip install -r requirements.txt
```

## Data

The Copyright holder of the dataset is [China Knowledge
Centre for Engineering Sciences and Technology (CKCEST)](http://zcy.ckcest.cn/tcm/). The dataset is for research use only. Any commercial use, sale, or other monetization is prohibited.

Training, validation and test records are in ./TCMdata/train.txt, ./TCMdata/val.txt and ./TCMdata/test.txt

Six example external unlabeled clinical records are in ./TCMdata/domain_corpus.txt. Due to [CKCEST](http://zcy.ckcest.cn/tcm/) policy, we could not provide all 46,205 records. But we provide our fine-tuned models.

The fine-tuned model from the second step is [here](https://drive.google.com/file/d/1VKKbfuzIdPwwgbYKSXBvhV7Ak1CSggSO/view?usp=sharing). The final fine-tuned text classifier is [here](https://drive.google.com/file/d/19y-mvsZmWVJg8NO9ZxKrkKIHV-sC4NNW/view?usp=sharing).

## How to run
 
### 1. Language model fine-tuning

```shell
python3 simple_lm_finetuning.py 
--train_corpus ./TCMdata/domain_corpus.txt 
--bert_model bert-base-chinese 
--do_lower_case 
--output_dir finetuned_lm_domain_corpus/ 
--do_train
```

### 2. Final text classifier fine-tuning
```shell
python3 run_classifier.py 
--do_eval 
--do_predict 
--data_dir ./TCMdata 
--bert_model bert-base-chinese 
--max_seq_length 400 
--train_batch_size 32 
--learning_rate 2e-5 
--num_train_epochs 3.0 
--output_dir ./output 
--gradient_accumulation_steps 16 
--task_name demo  
--do_train 
--finetuned_model_dir ./finetuned_lm_domain_corpus
```

## Reproducing results

1. Downloading the [fine-tuned language model](https://drive.google.com/file/d/1VKKbfuzIdPwwgbYKSXBvhV7Ak1CSggSO/view?usp=sharing).
2. Uncompressing the zip file in current folder.
3. Running the final text classifier fine-tuning.
4. The results of BERT can be reproduced by running the final text classifier fine-tuning without ```--finetuned_model_dir ./finetuned_lm_domain_corpus```.
