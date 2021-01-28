# speechocean762: A non-native English corpus for pronunciation scoring task


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
├── scores.json
├── scores-detail.json
├── train
│   ├── spk2age
│   ├── spk2gender
│   ├── spk2utt
│   ├── text
│   ├── utt2spk
│   └── wav.scp
├── test
│   ├── spk2age
│   ├── spk2gender
│   ├── spk2utt
│   ├── text
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

There are two datasets: `train` and `test`, and both are in Kaldi's data directory style.

The scores are stored in `scores.json`. Here is an example:

```json
{
    "000010011": {                                     # utt-id
        "text": "WE CALL IT BEAR",                     # transcript text
        "accuracy": 8,                                 # sentence-level accuracy score
        "completeness": 10.0,                          # sentence-level completeness score
        "fluency": 9,                                  # sentence-level fluency score
        "prosodic": 9,                                 # sentence-level prosodic score
        "total": 8,                                    # sentence-level total score
        "words": [
            {
                "accuracy": 10,                        # word-level accuracy score
                "stress": 10,                          # word-level stress score
                "total": 10,                           # word-level total score
                "text": "WE",                          # the word text
                "phones": "W IY0",                     # phones of the word                        
                "phones-accuracy": [2.0, 2.0]          # phoneme-level accuracy score
            },
            {
                "accuracy": 10,
                "stress": 10,
                "total": 10,
                "text": "CALL",
                "phones": "K AO0 L",
                "phones-accuracy": [2.0, 1.8, 1.8]
            },
            {
                "accuracy": 10,
                "stress": 10,
                "total": 10,
                "text": "IT",
                "phones": "IH0 T",
                "phones-accuracy": [2.0, 2.0]
            },
            {
                "accuracy": 6,
                "stress": 10,
                "total": 6,
                "text": "BEAR",
                "phones": "B EH0 R",
                "phones-accuracy": [2.0, 1.0, 1.0]
            }
        ]
    },
    ...
}
```

The file `scores.json` are processed from `scores-detail.json`.
The two JSON files are almostly same, but `scores-detail.json` has the original scores of the five experts,
while the scores of `scores.json` was the average or median scores.

An example item in `scores-detail.json`:
```json
{
    "000010011": {

        "text": "WE CALL IT BEAR",
        "accuracy": [7.0, 9.0, 8.0, 8.0, 9.0],
        "completeness": [1.0, 1.0, 1.0, 1.0, 1.0],
        "fluency": [10.0, 9.0, 8.0, 8.0, 10.0],
        "prosodic": [10.0, 9.0, 7.0, 8.0, 9.0],
        "total": [7.6, 9.0, 7.9, 8.0, 9.1],
        "words": [
            {
                "accuracy": [10.0, 10.0, 10.0, 10.0, 10.0],
                "stress": [10.0, 10.0, 10.0, 10.0, 10.0],
                "total": [10.0, 10.0, 10.0, 10.0, 10.0],
                "text": "WE",
                "ref-phones": "W IY0",
                "phones": ["W IY0", "W IY0", "W IY0", "W IY0", "W IY0"]
            },
            {
                "accuracy": [10.0, 8.0, 10.0, 10.0, 8.0],
                "stress": [10.0, 10.0, 10.0, 10.0, 10.0],
                "total": [10.0, 8.4, 10.0, 10.0, 8.4],
                "text": "CALL",
                "ref-phones": "K AO0 L",
                "phones": ["K AO0 L", "K {AO0} L", "K AO0 L", "K AO0 L", "K AO0 {L}"],
            },
            {
                "accuracy": [10.0, 10.0, 10.0, 10.0, 10.0],
                "stress": [10.0, 10.0, 10.0, 10.0, 10.0],
                "total": [10.0, 10.0, 10.0, 10.0, 10.0],
                "text": "IT",
                "ref-phones": "IH0 T",
                "phones": ["IH0 T", "IH0 T", "IH0 T", "IH0 T", "IH0 T"]
            },
            {
                "accuracy": [3.0, 7.0, 10.0, 2.0, 6.0],
                "stress": [10.0, 10.0, 10.0, 10.0, 10.0],
                "phones": ["B (EH0) (R)", "B {EH0} {R}", "B EH0 R", "B (EH0) (R)", "B EH0 [L] R"],
                "total": [4.4, 7.6, 10.0, 3.6, 6.8],
                "text": "BEAR",
                "ref-phones": "B EH0 R"
            }
        ],
    },
    ...
}
```

In `scores-detail.json`, the phoneme-level scores are notated in the following convenient notation:

* for score 2, do not use any symbol
* for score 1, use "{}" symbol
* for score 0, use "()" symbol
* for the inserted phone, use the "[]" symbol


For example, "B (EH) R" means the score of EH is 0 while the scores of B and R are both 2,
"B EH [L] R" mean there is an unexpected phone "L" and the other phones are scored 2.
