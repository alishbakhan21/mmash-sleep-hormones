# Sleep hormones in the MMASH dataset

I'm a B.Sc. Biotechnology student at Jamia Millia Islamia, New Delhi. I did this small project to learn how to work with human physiological data in Python, and to understand what a simple sampling design can and can't tell you about hormone rhythms.

The full analysis is in `01_load_and_merge.ipynb`. You can open it in Google Colab and run it from the top, since it downloads the data itself.

## Question

Do saliva melatonin and cortisol change between bedtime and waking? And are they related to chronotype or sleep quality?

## Data

I used the MMASH dataset, which has 24-hour recordings from 22 healthy young adults. Each person gave two saliva samples at home: one just before going to sleep and one right after waking up. One participant had no saliva data, so my analysis includes 21 people.

## What I did

- Loaded and merged the saliva, questionnaire and sleep files for all participants using pandas
- Reshaped the data so each person is one row, with separate bedtime and wake values
- One participant had two sleep periods, so I kept the longer one as their main sleep
- Compared bedtime and wake hormone levels with a Wilcoxon signed-rank test, because the data is paired, the sample is small, and there are a few extreme values
- Used Spearman correlations to look at chronotype (MEQ score) and sleep efficiency
- I only tested 3 correlations, decided before looking at the results, so I wouldn't find something just by chance

## Results

**Cortisol was higher at waking than at bedtime** in 18 of 21 people. The median was about 3 times higher (p = 0.0001). This fits the cortisol awakening response, where cortisol rises sharply soon after waking.

**Melatonin did not differ** between bedtime and waking (8 of 21 higher at waking, p = 0.32).

**I found no evidence of a link** between chronotype and bedtime melatonin (rho = 0.20, p = 0.40), chronotype and wake cortisol (rho = 0.18, p = 0.43), or sleep efficiency and bedtime melatonin (rho = 0.00, p = 0.99).

## Limitations

- With only two samples per person, both time points miss the night-time melatonin peak. To see the actual melatonin rhythm you would need repeated samples through the evening and night under dim light.
- Samples were collected at home, so light exposure wasn't controlled. Evening light is known to suppress melatonin, which could hide differences.
- 21 people is a small sample, so only strong effects could be picked up. "No evidence of a link" here doesn't mean there is no link.
- Most participants were intermediate chronotypes (15 intermediate, 4 morning, 2 evening), so I used the MEQ score as a number instead of comparing groups.
- One wake melatonin value was exactly 0, which is probably below the detection limit. I kept it and flagged it rather than removing it.

## What I learned

The cortisol result showed me that two well-timed samples can capture a strong, fast response. The melatonin result showed me the opposite: a rhythm that changes over the night can't be measured with two uncontrolled samples. Getting a reliable melatonin measurement depends heavily on controlling light and timing the samples carefully, and that's the part of human physiology research I'd most like to learn properly.

## Tools

Python (pandas, SciPy, matplotlib), Google Colab

## Data source

Rossi, A., Da Pozzo, E., Menicagli, D., Tremolanti, C., Priami, C., Sirbu, A., Clifton, D., Martini, C., & Morelli, D. (2020). A Public Dataset of 24-h Multi-Levels Psycho-Physiological Responses in Young Healthy Adults. *Data*, 5(4), 91. https://doi.org/10.3390/data5040091

Dataset: Multilevel Monitoring of Activity and Sleep in Healthy People (version 1.0.0), PhysioNet. https://doi.org/10.13026/cerq-fc86

The data is shared under the Open Database License (ODbL) v1.0. I haven't stored the raw data in this repository; the notebook downloads it from PhysioNet.
