# MERBench: A Unified Evaluation Benchmark for Multimodal Emotion Recognition

Correspondence to: 

  - Zheng Lian: lianzheng2016@ia.ac.cn
  - Licai Sun: sunlicai2019@ia.ac.cn



## Paper

[**MERBench: A Unified Evaluation Benchmark for Multimodal Emotion Recognition**](https://arxiv.org/pdf/2401.03429)<br>
Zheng Lian, Licai Sun, Yong Ren, Hao Gu, Haiyang Sun, Lan Chen, Bin Liu, Jianhua Tao<br>

Please cite our paper if you find our work useful for your research:

```tex
@article{lian2023mer,
  title={MERBench: A Unified Evaluation Benchmark for Multimodal Emotion Recognition},
  author={Lian, Zheng and Sun, Licai and Ren, Yong and Gu, Hao and Sun, Haiyang and Chen, Lan and Liu, Bin and Tao, Jianhua},
  journal={arXiv:2401.03429},
  year={2024}
}
```

paper: https://arxiv.org/pdf/2401.03429



## Usage
This is an extension of **./MER2023**. You can also refer to **./MER2023** for more details.


### Create ./tools folder

```shell
## for face extractor (OpenFace-win)
https://drive.google.com/file/d/1-O8epcTDYCrRUU_mtXgjrS3OWA4HTp0-/view?usp=share_link  -> tools/openface_win_x64
## for visual feature extraction
https://drive.google.com/file/d/1DZVtpHWXuCmkEtwYJrTRZZBUGaKuA6N7/view?usp=share_link ->  tools/ferplus
https://drive.google.com/file/d/1wT2h5sz22SaEL4YTBwTIB3WoL4HUvg5B/view?usp=share_link ->  tools/manet
https://drive.google.com/file/d/1-U5rC8TGSPAW_ILGqoyI2uPSi2R0BNhz/view?usp=share_link ->  tools/msceleb

## for audio extraction
https://www.johnvansickle.com/ffmpeg/old-releases ->  tools/ffmpeg-4.4.1-i686-static
## for acoustic acoustic features
https://drive.google.com/file/d/1I2M5ErdPGMKrbtlSkSBQV17pQ3YD1CUC/view?usp=share_link ->  tools/opensmile-2.3.0
https://drive.google.com/file/d/1Q5BpDrZo9j_GDvCQSN006BHEuaGmGBWO/view?usp=share_link ->  tools/vggish

## huggingface for multimodal feature extracion
## We take chinese-hubert-base for example, all pre-trained models are downloaded to tools/transformers. The links for different feature extractos involved in MERBench, please refer to Table18 in our paper.
https://huggingface.co/TencentGameMate/chinese-hubert-base    -> tools/transformers/chinese-hubert-base
```



### Dataset Preprocessing

(1) You should download the raw datasets.

(2) We provide the code for dataset preprocessing.

```shell
# please refer to toolkit/proprocess for more details
see toolkit/proprocess/mer2023.py 
see toolkit/proprocess/sims.py
see toolkit/proprocess/simsv2.py
see toolkit/proprocess/cmumosi.py
see toolkit/proprocess/cmumosei.py
see toolkit/proprocess/meld.py
see toolkit/proprocess/iemocap.py
```

(3) Feature extractions

Please refer to **run.sh** for more details.

You can choose feature_level in ['UTTERANCE', 'FRAME'] to extract utterance-level or frame-level features.

You can choose '--dataset' in ['MER2023', 'IEMOCAPSix', 'CMUMOSI', 'CMUMOSEI', 'SIMS', 'MELD', 'SIMSv2'] to extract features for different datasets.

```shell
# visual features
1. extract face using openface
cd feature_extraction/visual
python extract_openface.py --dataset=MER2023 --type=videoOne

2. extract visual features
python -u extract_vision_huggingface.py --dataset=MER2023 --feature_level='UTTERANCE' --model_name='clip-vit-large-patch14'           --gpu=0    
python -u extract_vision_huggingface.py --dataset=MER2023 --feature_level='FRAME' --model_name='clip-vit-large-patch14'           --gpu=0    

# lexical features
python extract_text_huggingface.py --dataset='MER2023' --feature_level='UTTERANCE' --model_name='Baichuan-13B-Base'                     --gpu=0  
python extract_text_huggingface.py --dataset='MER2023' --feature_level='FRAME' --model_name='Baichuan-13B-Base'                     --gpu=0  

# acoustic features
1. extract 16kHZ audio from videos
python toolkit/utils/functions.py func_split_audio_from_video_16k 'dataset/sims-process/video' 'dataset/sims-process/audio'

2. extract acoustic features
python -u extract_audio_huggingface.py     --dataset='MER2023' --feature_level='UTTERANCE' --model_name='chinese-hubert-large'     --gpu=0
python -u extract_audio_huggingface.py     --dataset='MER2023' --feature_level='FRAME' --model_name='chinese-hubert-large'     --gpu=0
```



For convenience, we provide processed labels and features in **./dataset** folder.

 Since features are relatively large, we upload them into Baidu Cloud Disk:

```
store path: ./dataset/mer2023-dataset-process   link: https://pan.baidu.com/s/1l2yrWG3wXHjdRljAk32fPQ         password: uds2 
store path: ./dataset/simsv2-process 			link: https://pan.baidu.com/s/1oJ4BP9F4s2c_JCxYVVy1UA         password: caw3 
store path: ./dataset/sims-process   			link: https://pan.baidu.com/s/1Sxfphq4IaY2K0F1Om2wNeQ         password: 60te 
store path: ./dataset/cmumosei-process  		link: https://pan.baidu.com/s/1GwTdrGM7dPIAm5o89XyaAg         password: 4fed 
store path: ./dataset/meld-process   			link: https://pan.baidu.com/s/13o7hJceXRApNsyvBO62FTQ         password: 6wje 
store path: ./dataset/iemocap-process   		link: https://pan.baidu.com/s/1k8VZBGVTs53DPF5XcvVYGQ         password: xepq 
store path: ./dataset/cmumosi-process   		link: https://pan.baidu.com/s/1RZHtDXjZsuHWnqhfwIMyFg         password: qnj5 
```



### Unimodal Benchmark

1. You can choose '--dataset' in ['MER2023', 'IEMOCAPSix', 'IEMOCAPFour', 'CMUMOSI', 'CMUMOSEI', 'SIMS', 'MELD', 'SIMSv2']
2. You can also change the feature names, we take three unimodal features for example.
2. By default, we randomly select hyper-parameters during training. Therefore, please run each command line 50 times, choose the best hyper-parameters, run 6 times and calculate the average result.

~~~~shell
python -u main-release.py --model='attention' --feat_type='utt' --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='chinese-hubert-large-UTT' --video_feature='chinese-hubert-large-UTT' --gpu=0

python -u main-release.py --model='attention' --feat_type='utt' --dataset='MER2023' --audio_feature='clip-vit-large-patch14-UTT' --text_feature='clip-vit-large-patch14-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0

python -u main-release.py --model='attention' --feat_type='utt' --dataset='MER2023' --audio_feature='Baichuan-13B-Base-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='Baichuan-13B-Base-UTT' --gpu=0
~~~~



### Multimodal Benchmark

We provide 5 utterance-level fusion algorithms and 5 frame-level fusion algorithms.

```shell
## for utt-level fusion
python -u main-release.py --model='attention'   --feat_type='utt'         --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0
python -u main-release.py --model='lmf'         --feat_type='utt'         --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0
python -u main-release.py --model='misa'        --feat_type='utt'         --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0
python -u main-release.py --model='mmim'        --feat_type='utt'         --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0
python -u main-release.py --model='tfn'         --feat_type='utt'         --dataset='MER2023' --audio_feature='chinese-hubert-large-UTT' --text_feature='Baichuan-13B-Base-UTT' --video_feature='clip-vit-large-patch14-UTT' --gpu=0

## for frm_align fusion
python -u main-release.py --model='mult'        --feat_type='frm_align'   --dataset='MER2023' --audio_feature='chinese-hubert-large-FRA' --text_feature='Baichuan-13B-Base-FRA' --video_feature='clip-vit-large-patch14-FRA' --gpu=0
python -u main-release.py --model='mfn'         --feat_type='frm_align'   --dataset='MER2023' --audio_feature='chinese-hubert-large-FRA' --text_feature='Baichuan-13B-Base-FRA' --video_feature='clip-vit-large-patch14-FRA' --gpu=0
python -u main-release.py --model='graph_mfn'   --feat_type='frm_align'   --dataset='MER2023' --audio_feature='chinese-hubert-large-FRA' --text_feature='Baichuan-13B-Base-FRA' --video_feature='clip-vit-large-patch14-FRA' --gpu=0
python -u main-release.py --model='mfm'         --feat_type='frm_align'   --dataset='MER2023' --audio_feature='chinese-hubert-large-FRA' --text_feature='Baichuan-13B-Base-FRA' --video_feature='clip-vit-large-patch14-FRA' --gpu=0
python -u main-release.py --model='mctn'        --feat_type='frm_align'   --dataset='MER2023' --audio_feature='chinese-hubert-large-FRA' --text_feature='Baichuan-13B-Base-FRA' --video_feature='clip-vit-large-patch14-FRA' --gpu=0
```



### Cross-corpus Benchmark

We provide both unimodal and multimodal cross-corpus benchmarks:

Please change **--train_dataset** and **--test_dataset** for cross-corpus settings.

```shell
## test for sentiment strength, we take SIMS -> CMUMOSI for example
python -u main-release.py --model=attention --feat_type='utt' --train_dataset='SIMS' --test_dataset='CMUMOSI'  --audio_feature=Baichuan-13B-Base-UTT    --text_feature=Baichuan-13B-Base-UTT --video_feature=Baichuan-13B-Base-UTT      --gpu=0
python -u main-release.py --model=attention --feat_type='utt' --train_dataset='SIMS' --test_dataset='CMUMOSI'  --audio_feature=chinese-hubert-large-UTT --text_feature=Baichuan-13B-Base-UTT --video_feature=clip-vit-large-patch14-UTT --gpu=0

## test for discrete labels, we take MER2023 -> MELD for example
python -u main-release.py --model=attention --feat_type='utt' --train_dataset='MER2023' --test_dataset='MELD'  --audio_feature=Baichuan-13B-Base-UTT    --text_feature=Baichuan-13B-Base-UTT --video_feature=Baichuan-13B-Base-UTT      --gpu=0
python -u main-release.py --model=attention --feat_type='utt' --train_dataset='MER2023' --test_dataset='MELD'  --audio_feature=chinese-hubert-large-UTT --text_feature=Baichuan-13B-Base-UTT --video_feature=clip-vit-large-patch14-UTT --gpu=0
```



## Acknowledgement

Thanks to [Hugging Face](https://huggingface.co/docs/transformers/index), [MMSA](https://github.com/thuiar/MMSA), [pytorch](https://github.com/pytorch/pytorch), [openface](https://github.com/TadasBaltrusaitis/OpenFace), [fairseq](https://github.com/facebookresearch/fairseq)
