# MI-1: A non-native English corpus for pronunciation scoring task


## Introduction
Pronunciation scoring is an essensial technology in computer-assisted language learning (CALL) systems.
For a typical pronunciation scoring task, the prounciation quality scores might be given at phoneme-level, word-level and sentence-level.
However, no scoring metric was widely accepted, and there is not any public dataset for this task.
Researches use their own private datasets in their papers.

This corpus aims to provide a free public dataset for the pronunciation scoring task.

This corpus consists 5000 English sentences.
All the speakers are non-native and their mother tongue is Mandarin.
Half of the speakers are Children and the others are adults.
The information of age and gender are provided.

The scores was made by five experts. To avoid subjectively bias, each experts scores independently under the same metric.


## The scoring metric
The experts score at three levels: phoneme-level, word-level and sentence-level.

### Phoneme level
Score the pronunciation goodness of each phoneme within the words.

Score range: 0-2
* 2: pronunciation is correct
* 1: pronunciation is correct but has heavy accent
* 0: pronunciation is incorrect or missed

### Word level
Score the accuracy and stress of each word's pronunciation.

#### Accuracy
Score range: 0 - 10
* 10: xxxx
* 7-9: xxxx
* 4-6: xxxx
* 2-3: xxxx
* 1: xxxx
* 0: no voice

#### Stress
Score range: 5 - 10
* 10: stress correct or this is a mono-syllable word
* 5: stress wrong

### Sentence level
Score the accuracy, fluency, completeness and prosodic at the sentence level.

#### Accuracy
Score range: 0 - 10
* 9-10: xxxx
* 7-8: xxxx
* 5-6: xxxx
* 3-4: xxxx
* 0-2: xxxx

#### Completeness
Score range: 0.0 - 1.0
The percentage of the words with good pronunciation.

#### Fluency
Score range: 0 - 10
* 8-10: xxxx
* 6-7: xxxx
* 4-5: xxxx
* 0-3: xxxx

## Data structure
The following tree shows the file structure of this corpus:
```
├── scripts
│   ├── spk2age
│   ├── spk2gender
│   ├── spk2utt
│   ├── text
│   ├── utt2score
│   ├── utt2spk
│   └── wav.scp
└── WAVE
    ├── SPEAKER0001
    │   ├── 000010011.WAV
    │   ├── 000010035.WAV
    │   ├── ...
    ├── SPEAKER0003
    │   ├── 000030012.WAV
    │   ├── 000030024.WAV
    │   ├── 000030040.WAV
    │   ├── ...
    └── SPEAKER0005
        ├── 000050003.WAV
        ├── 000050010.WAV
        ├── 000050024.WAV
        ├── ...
```

Most files in `scripts` use Kaldi's data style.
The most important file is `utt2score`, whose format is similar with `text`, while the second column of each line is a JSON:

```
000010011	{"text": "we call it bear", "total": [7.6, 9.0, 7.9, 6.4, 9.1], "accuracy": [7,  ... }
000010053	{"text": "three two two seven", "total": [7.6, 10.0, 8.9, 9.0, 9.2], "accuracy": ... }
000010063	{"text": "elephants tai goose", "total": [10.0, 9.8, 8.9, 8.2, 9.9], "accuracy": ... }
```


### An example of JSON line in `utt2score`
```json
{
    "text": "we call it bear",
    "total": [7.6, 9, 7.9, 6.4, 9.1],
    "accuracy": [7, 9, 8, 8, 9],
    "completeness": [1, 1, 1, 0.75, 1],
    "fluency": [10, 9, 8, 8, 10],
    "prosodic": [10, 9, 7, 8, 9],
    "words": [
        {
            "text": "we",
            "total": [10, 10, 10, 10, 10],
            "accuracy": [10, 10, 10, 10, 10],
            "stress": [10, 10, 10, 10, 10],
            "phones": ["W IY",
                       "W IY",
                       "W IY",
                       "W IY",
                       "W IY"
           ]
        },
        {
            "text": "call",
            "total": [10, 8.4, 10, 10, 8.4],
            "accuracy": [10, 8, 10, 10, 8],
            "stress": [10, 10, 10, 10, 10],
            "phones": [
                "K AO L",
                "K {AO} L",
                "K AO L",
                "K AO L",
                "K AO {L}"
           ]
        },
        {
            "text": "it",
            "total": [10, 10, 10, 10, 10],
            "accuracy": [10, 10, 10, 10, 10],
            "stress": [10, 10, 10, 10, 10],
            "phones": [
                "IH T",
                "IH T",
                "IH T",
                "IH T",
                "IH T"
           ]
        },
        {
            "text": "bear",
            "total": [4.4, 7.6, 10, 3.6, 6.8],
            "accuracy": [3, 7, 10, 2, 6],
            "stress": [10, 10, 10, 10, 10],
            "phones": [
                "B (EH) R",
                "B EH {R}",
                "B AR",
                "B (AR)",
                "B EH [L] R"
           ]
        }
   ]
}
```

The phoneme-level scores are notated in the following convenient notation:

* for score 2, do not use any symbol
* for score 1, use "{}" symbol
* for score 0, use "()" symbol
* for the inserted phone, use the "[]" symbol


For example, "B (EH) R" means the score of EH is 0 while the scores of B and R are both 2,
"B EH [L] R" mean there is an unexpected phone "L" and the other phones are scored 2.
