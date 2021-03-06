A Pytorch implementation of 'AUTOMATIC SPEECH EMOTION RECOGNITION USING RECURRENT NEURAL NETWORKS WITH LOCAL ATTENTION' 
===


[paper](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=2ahUKEwipkIKO2sPcAhVVMd4KHY4eC2MQFjABegQIARAC&url=https%3A%2F%2Fsigport.org%2Fsites%2Fdefault%2Ffiles%2Fdocs%2Ficassp2017_1.pdf&usg=AOvVaw0GCE0gJV_TEWKZueyQfy9T)

[IEMOCAP DB paper](https://pdfs.semanticscholar.org/5cf0/d213f3253cd46673d955209f8463db73cc51.pdf)

[MSP-IMPROV DB paper](https://web.eecs.umich.edu/~emilykmp/EmilyPapers/2016_Busso_TAFF.pdf)

## Requirements

- Python 3.6.4

- Pytorch 0.4.1

- [opensmile 2.3.0](https://www.audeering.com/research/opensmile)

- [fileutils.readHtk](https://github.com/MaigoAkisame/fileutils)(githubrepo. I changed htk.py for python3)


## Preparation

### wav_cat.list, utt.list

IEMOCAP DB has 5531 utterances, composed of 4 Emotions.

A: Anger
H: Excited + Happiness
N: Neutral
S: Sadness

```bash
#head -2 iemocap/wav_cat.list
/your/path/Ses01F_impro01_F000.wav N
/your/path/Ses01F_impro01_F001.wav N

#head -2 iemocap/utt.list
Ses01F_impro01_F000
Ses01F_impro01_F001

```

MSP-IMPROV DB has 7798 utterances, composed of 4 Emotions.

```bash
#head -2 msp_improv/wav_cat.list
/your/path/MSP-IMPROV-S01A-F01-P-FM01.wav N 
/your/path/MSP-IMPROV-S01A-F01-P-FM02.wav H

#head -2 msp_improv/utt.list
MSP-IMPROV-S01A-F01-P-FM01
MSP-IMPROV-S01A-F01-P-FM02
```

## How to Run

```bash
./add_opensmile_conf.sh your_opensmile_dir

./prepare_list.sh iemocap/wav_cat.list \ # done.
	iemocap/lld.htk.list iemocap/utt.list iemocap/lld/

./extract_lld.sh your_opensmile_dir/ iemocap/wav_cat.list \
	iemocap/lld.htk.list

./make_utt_lld_pair.py iemocap/utt.list iemocap/lld.htk.list \
	iemocap/utt_lld.pk

./iemocap/make_csv.sh iemocap/utt.list iemocap/wav_cat.list iemocap/ \
	iemocap/full_dataset.csv

# Modify make_dataset.py parameters as you want!
#
### Default setting ###
#
# devfrac=0.2
# session=1
# prelabel="gender"
#
# e.g.
# sed 's/"gender"/"speaker"/' iemocap/make_dataset.py > new_script.py
# sed 's/devfrac=0.2/devfrac=0.1/' iemocap/make_dataset.py > new_script.py

./iemocap/make_dataset.py iemocap/full_dataset.csv iemocap/utt_lld.pk iemocap/your_dataset_path

# Modify make_expcase.py params as you want!
#
### Default setting ###
#
# lr=0.00005
# bsz=64
# ephs=200

./iemocap/make_expcase.py iemocap/your_dataset_path iemocap/your_dataset_path/your_expcase

#ls iemocap/your_dataset_path/your_expcase 

# log	
# param.json
# premodel.pth
# model.pth

./run.py --propjs iemocap/your_dataset_path/your_expcase/param.json

#grep test iemocap/your_dataset_path/your_expcase/log

#iemocap/sess1/exp/log:[test] score: 0.451, loss: 1.282
#iemocap/sess2/exp/log:[test] score: 0.535, loss: 1.201
#iemocap/sess3/exp/log:[test] score: 0.609, loss: 1.131
#iemocap/sess4/exp/log:[test] score: 0.560, loss: 1.177
#iemocap/sess5/exp/log:[test] score: 0.505, loss: 1.233
```



