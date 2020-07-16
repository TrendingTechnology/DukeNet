# DukeNet (SIGIR 2020 full paper)
The code for [DukeNet: A Dual Knowledge Interaction Network for Knowledge-Grounded Conversation]()
![image](https://github.com/ChuanMeng/DukeNet/blob/master/figure.png)

## Reference
If you use any source code included in this repo in your work, please cite the following paper.
```
@inproceedings{chuanmeng2020dukenet,
 author = {Meng, Chuan and Ren, Pengjie and Chen, Zhumin and Sun, Weiwei and Ren, Zhaochun and Tu, Zhaopeng and de Rijke, Maarten},
 booktitle = {SIGIR},
 title = {DukeNet: A Dual Knowledge Interaction Network for Knowledge-Grounded Conversation},
 year = {2020}
}
```

## Requirements 
* python 3.6
* pytorch 1.2.0-1.4.0
* transformers

## Datasets
We use Wizard of Wikipedia and Holl-E datasets. Note that we used modified verion of Holl-E relased by [Kim et al](https://arxiv.org/abs/2002.07510?context=cs.CL) (They don't release the validation set).
Both datasets have already been processed into our defined format, which could be directly used by our model.

You can manually download the datasets at [here](), and please put the files in the directory `DukeNet/datasets`.

## Running Codes
Note that it's rather **time-consuming** to train the DukeNet with BERT and dual learning. Therefore we upload our pretrained checkpoints on two datasets, and you can manually download them at [here]().

Please put the files in the directory `DukeNet/output`.

### Using pretrained checkpoints
To directly execute inference process on **Wizard of Wikipedia** dataset, run:
```
python DukeNet/Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia --mode inference

Wizard of Wikipedia (test seen)
{"F1": 19.33,
 "BLEU-1": 17.99,
 "BLEU-2": 7.51,
 "BLEU-3": 4.04,
 "BLEU-4": 2.46,
 "ROUGE_1_F1": 25.38,
 "ROUGE_2_F1": 6.83,
 "ROUGE_L_F1": 18.67,
 "METEOR": 17.23,
 "t_acc": 81.9,
 "s_acc": 25.58}

Wizard of wikipedia (test unseen)
{"F1": 17.09,
 "BLEU-1": 16.34,
 "BLEU-2": 5.99,
 "BLEU-3": 2.85,
 "BLEU-4": 1.69,
 "ROUGE_1_F1": 23.45,
 "ROUGE_2_F1": 5.31,
 "ROUGE_L_F1": 16.95,
 "METEOR": 15.22,
 "t_acc": 75.88,
 "s_acc": 20.07}
```
**Note**: The inference process will create `test_seen_result.json(test_unseen_result.json)` and `test_seen_xx.txt(test_unseen_xx.txt)` in the directory `output/DukeNet_WoW`, where the former records the model performance about automatic evaluation metrics for epochs, and the latter records the response generated by the model.

To directly execute inference process on **Holl-E** dataset, run:
```
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode inference

Holl-E (single golden reference)
{"F1": 30.32,
 "BLEU-1": 29.97, 
 "BLEU-2": 22.2, 
 "BLEU-3": 20.09, 
 "BLEU-4": 19.15, 
 "ROUGE_1_F1": 36.53, 
 "ROUGE_2_F1": 23.02, 
 "ROUGE_L_F1": 31.46, 
 "METEOR": 30.93, 
 "t_acc": 92.11, 
 "s_acc": 30.03}
 ```
 
If you want to get the results in the setting of multiple golden references, run:
```
python DukeNet/CumulativeTrainer.py 

Holl-E (multiple golden references)
{"F1": 37.31,
 "BLEU-1": 40.36, 
 "BLEU-2": 30.78, 
 "BLEU-3": 27.99, 
 "BLEU-4": 26.83, 
 "ROUGE_1_F1": 43.18, 
 "ROUGE_2_F1": 30.13, 
 "ROUGE_L_F1": 38.03, 
 "METEOR": 37.73,  
 "s_acc": 40.33}
```

### Retraining
To execute warm-up training phase and dual interaction training phase, run sequentially:
```
Wizard of Wikipedia
python DukeNet/Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia --mode inference 
python DukeNet/Dual_Run.py --name DukeNet_WoW --dataset wizard_of_wikipedia 

Holl-E
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode train
python DukeNet/Dual_Run.py --name DukeNet_Holl_E --dataset holl_e 
```
You can run a inference job after the training, or at the same time with training:
```
Wizard of Wikipedia
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode inference

Holl-E
python DukeNet/Run.py --name DukeNet_Holl_E --dataset holl_e --mode inference
```








