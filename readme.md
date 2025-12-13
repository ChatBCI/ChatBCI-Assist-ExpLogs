# ChatBCI-Assist Experiment-Logs



This repository contains the **real-time experimental logs** collected from the 2025 **ChatBCI-Assist** study.  
The system includes **10 subjects**, each completing **3 online sessions**, and each session containing **3 typing tasks** (T1–T3). For the specific sentences used in each typing task, please see the [Sentence summary](#sentence-summary) section at the end of this README.

ChatBCI is a P300 speller brain–computer interface (BCI)  integrated with large language models (LLMs).  
Depending on the online session type, the backend may be:

- **Dictionary-based predictive spelling** (Copy-Baseline)
- **LoRA-finetuned Llama-based word/phrase prediction** (Copy-LLM)
- **Semantic-level LLM intent-driven phrase prediction** (Semantic-LLM)

User experience was assessed after each online session using standardized subjective questionnaires, including NASA-TLX, VAS-F, and assistive technology usability scales.
[![ChatBCI-Assist Questionnaires](https://img.shields.io/badge/Live%20App-Streamlit-brightgreen?logo=streamlit)](https://chatbci-assist-questionnaires-live.streamlit.app/)


## 🎥 System Demonstration

A real-time demonstration of the **ChatBCI-Assist Semantic-LLM speller**
can be viewed here:

👉 **[Watch the Demo Video](https://chatbci.github.io/ChatBCI-Assist-ExpLogs/)**

(The video plays directly in the browser via GitHub Pages.)


These logs represent real-time **in-vivo closed-loop BCI spelling behaviors**, including character/word selections, suggestions, and sentence evolution.

---

# 📁 Folder Structure

```
ChatBCI-Experiment-Logs/
├── S01/
│   ├── S01-O1T1/
│   │   ├── S01-O1T1-textLog.txt
│   │   ├── S01-O1T1-wordLog.txt
│   │   └── S01-O1T1-selectLog.txt
│   ├── S01-O1T2/
│   ├── S01-O1T3/
│   ├── S01-O2T1/
│   ├── S01-O2T2/
│   ├── S01-O2T3/
│   ├── S01-O3T1/
│   ├── S01-O3T2/
│   └── S01-O3T3/
├── S02/
├── …
├── S10/
└── README.md
```

### Naming Convention

A folder name such as: `S01-O2T3`

means:

| Component | Meaning |
|----------|---------|
| `S01` | Subject ID |
| `O1` | Online Session 1 (Copy-Baseline) |
| `O2` | Online Session 2 (Copy-LLM) |
| `O3` | Online Session 3 (Semantic-LLM) |
| `T1` | Typing Task 1 (Database sentences)
| `T2` | Typing Task 2 (Database sentences)
| `T3` | Typing Task 3 (Self-originated sentences) 

Each task folder contains **three log files**:

| File | Description |
|------|-------------|
| `textLog.txt` | Sentence evolution after each spelling cycle |
| `wordLog.txt` | List of candidate suggestions shown to user |
| `selectLog.txt` | Actual user selections (letters, words, phrases) |

---

# 🧪 Experiment Design Summary

Each subject completed **3 online sessions**, each containing **3 typing tasks** (T1–T3).  
The order of the three sessions was **randomized** for each subject to minimize fatigue or session-order bias.

### Typing Tasks

- **T1 & T2** – database sentences (not used during LLM training)  
- **T3** – self-originated sentence based on a randomly selected topic  

### Sentence Pool

- 20 total target sentences
  - 10 sampled from testing dataset (10 topics × 1 sentence each, used by 2 subjects)
  - 10 self-originated sentences created by subjects (topic randomly selected per subject)

---

# 🧩 Online Session Types

## **Online Session 1 — Copy-Baseline**

Uses a **dictionary-based predictive spelling** backend.

- Users copy-spell 3 target sentences **exactly**  
- The dictionary contains **846 unique words** (same training set as LLM)  
- **Word Completion (WC)** – find words matching prefix  
- **Word Prediction (WP)** – globally frequent words  
- GUI shows **6 suggestions**  
- `"+"` and `"*"` both insert the same single-word suggestion  
- Serves as the **baseline** reference

---

## **Online Session 2 — Copy-LLM**

Uses the **LoRA-finetuned Llama** model.

- Same copy-spelling task as Session 1  
- LLM produces **words** and **multi-word phrases**  
- `"+"` = word suggestion  
- `"*"` = phrase suggestion  
- Users are encouraged to maximize phrase-level usage to **reduce key selections**

---

## **Online Session 3 — Semantic-LLM**

Intent-driven spelling with the **same LLM backend**.

- Users do **not need** to match exact sentence wording  
- They express the **same meaning**, minimizing required selections  
- Reflects **naturalistic** communication  
- Evaluates LLM’s capability to interpret user intent from partial context

---

# 📝 Log File Descriptions

## 1. `textLog.txt`

Tracks the evolving composed sentence after each spelling cycle.

Example:
```
2025/11/01_01:38:46,I-WOULD-LIKE-A-BOWL-OF-SOUP-
2025/11/01_01:39:34,I-WOULD-LIKE-A-BO
2025/11/01_01:39:47,I-WOULD-LIKE-A-BOWL-
2025/11/01_01:41:00,I-WOULD-LIKE-A-BOWL-OF-
2025/11/01_01:41:54,I-WOULD-LIKE-A-BOWL-OF-SOUP-
```


Format:

- `[timestamp],[sentence_state]`
- Dash (`-`) represents a space  
- Optional tags like `[WRONG]` show user corrections  
- Each line = **state after a selection cycle**

---

## 2. `selectLog.txt`

Logs every user action (letter/word/phrase selection, delete, space).

Example:
```
2025/11/01_01:38:55,A
2025/11/01_01:39:13,DelChar
2025/11/01_01:39:47,Bw6
2025/11/01_01:40:18,DelWord
2025/11/01_01:41:11,S
```

Meaning of common tokens:

| Token | Meaning |
|-------|---------|
| `A`, `B`, `C`, ... | Letter selection |
| `Space` | Insert space |
| `Bw3` | Select 3rd suggestion (word or phrase) |
| `DelChar` | Delete last character |
| `DelWord` | Delete last word |

`Bw#` covers both *word* and *phrase* suggestions depending on the session backend.

---

## 3. `wordLog.txt`

Records the list of candidate suggestions shown to the user at each cycle.

Example:
```
2025/11/01_01:38:49,I,YOU,TO,IS,THAT,IT
2025/11/01_01:39:40,BOOK,BOTHERING,BOY,BOTH,BODY,BOWL
2025/11/01_01:41:48,SOUNDS,SOUND,SOUP,SOUL,SOUNDED,
```

Notes:

- Copy-Baseline → 6 single-word suggestions  
- Copy-LLM / Semantic-LLM → includes multi-word phrases where available  
- Can be aligned with `selectLog.txt` to reconstruct user behavior

---

### Note: Time Limit and Log Truncation

Each spelling task (T1–T3) is limited to **10 minutes**.  
If a task exceeds this time limit, the experiment is manually terminated. Due to the **dynamic-stopping flashing algorithm** in the P300 detection paradigm, the actual stop moment may not align perfectly with the 10-minute boundary, and the final timestamps may reflect slight offsets.

For consistency across subjects, we define the **last timestamp within the 10-minute window** as the effective end of the task.  
All log-based analyses (e.g., time to complete (T2C)) use this truncated endpoint as the valid stopping point.

# Sentence summary

The Boston Children's Hospital ALS Message Bank [1] contains 2,171 sentences covering 41 categories of typical communication demands from actual ALS patients. After removing short sentences containing fewer than 2 words and any non-alphabetical symbols, 38 topics and 1,974 cleaned sentences were retrieved. The cleaned sentences were randomly split into 1,384 training, 200 validation, and 394 testing sentences, following a 70%–10%–20% ratio. The validation sentences were held out for simulation-based model validation and subject practice. The testing sentences were reserved for online experiments.

During each online session, the subject was required to convey 3 different target sentences. These target sentences come from a pool of 20 sentences that were not used for training the LLM nor practiced by the subject. Among the 20 sentences, 10 were randomly sampled from the testing set to span 10 distinct topics, each assigned to two different subjects. The remaining 10 sentences were self-originated by the subjects from 10 randomly selected topics, to further evaluate the out-of-bag generalization capability of the system in different settings.


[1] J. M. Costello, “Message banking, voice banking and legacy messages,” Boston Children’s Hospital, Boston, MA, 2014.

|  ID |        Topics                                   | Sentences                                                    | Used in            |
|-------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------|
| -     | -                                         | THE-QUICK-BROWN-FOX-JUMPS-OVER-THE-LAZY-DOG                  | Calibration        |
| \# 9  | Conversation Modifiers/Repairs            | PLEASE-WAIT-UNTIL-I-FINISH-WHAT-I-AM-TRYING-TO-SAY           | Practice Session   |
| \# 24 | Nourishment/Food                          | I-AM-HUNGRY-FOR-SOME-BREAKFAST                               | Practice Session   |
| \# 32 | Occasions/Holidays/Celebrations           | I-AM-GLAD-I-COULD-BE-PART-OF-THE-CELEBRATION                 | Practice Session   |
| \# 34 | Self Determination                        | PLEASE-KEEP-IN-MIND-I-WAS-VERY-HEALTHY-JUST-A-SHORT-TIME-AGO | Practice Session   |
| \# 8  | Physical State Phrases                    | IT-IS-VERY-UNCOMFORTABLE-WHEN-IT-CRAMPS                      | Practice Session   |
| \# 24 | Nourishment/Food                          | I-AM-NOT-HUNGRY                                              | Validation Session |
| **Database sentences** ||| **Online Session**|
| \# 24 | Nourishment/Food                          | I-WOULD-LIKE-A-BOWL-OF-SOUP                                  | S01/02–O*T1 |
| \# 25 | Likes/Dislikes                            | I-DO-NOT-LIKE-THAT-WITH-MY-FOODS                             | S01/02–O*T2 |
| \# 7  | Equipment Related Phrases                 | TURN-IT-DOWN-A-LITTLE-BIT-PLEASE                             | S03/04–O*T1 |
| \# 15 | Location Marker Phrases                   | I-AM-READY-TO-GO-BACK                                        | S03/04–O*T2 |
| \# 9  | Ice Breaker (Conversation Opener) Phrases | DO-YOU-HAVE-ANY-BROTHERS-OR-SISTERS                          | S05/06–O*T1 |
| \# 11 | Goodbye/Farewell Phrases                  | IT-HAS-BEEN-A-PLEASURE-MEETING-YOU                           | S05/06–O*T2 |
| \# 8  | Physical State Phrases                    | PLEASE-MOVE-THE-PILLOW-UNDER-MY-NECK                         | S07/08–O*T1 |
| \# 4  | Time of Day Based Expressions             | I-WOULD-LIKE-TO-EAT-LUNCH                                    | S07/08–O*T2 |
| \# 30 | Compassion                                | I-AM-SORRY-YOU-HAVE-TO-GO-THROUGH-THIS-WITH-ME               | S09/10–O*T2 |
| \# 23 | Generic Responses Phrases                 | I-KNOW-I-DO-NOT-NEED-TO-BE-REMINDED                          | S09/10–O*T2 |
| **Self-originated sentences** |||**Online Session**|
| \# 8  | Physical State Phrases                    | MY-LOWER-BACK-HAS-BEEN-HURTING-A-LOT-THIS-PAST-WEEK          | S01-O*T3             |
| \# 17 | Temporal Markers                          | CAN-WE-SCHEDULE-A-MEETING-NEXT-WEEK                          | S02-O*T3             |
| \# 6  | Appointments                              | THE-KIDS-HAVE-A-DENTIST-APPOINTMENT-TODAY                    | S03-O*T3             |
| \# 12 | Request for Assistance                    | I-NEED-TO-GO-TO-THE-RESTROOM-AFTER-DRINKING-SO-MUCH-TEA      | S04-O*T3             |
| \# 8  | Physical State Phrases                    | TODAY-I-WAS-VERY-TIRED                                       | S05-O*T3             |
| \# 6  | Appontments                               | CAN-I-MAKE-AN-APPOINTMENT-WITH-MY-FAMILY-DOCTOR              | S06-O*T3             |
| \# 30 | Compassion                                | I-WOULD-LIKE-TO-HAVE-MY-MOTHER-BY-MY-SIDE                    | S07-O*T3             |
| \# 8  | Physical State Phrases                    | THIS-ROOM-IS-HOT-RIGHT-NOW                                   | S08-O*T3             |
| \# 2  | Social Requests                           | I-WOULD-LIKE-TO-CALL-MY-MOTHER                               | S09-O*T3             |
| \# 25   | Likes/Dislikes                            | I-REALLY-LIKE-GREEN-APPLES                                   | S10-O*T3             |
