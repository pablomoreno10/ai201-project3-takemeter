r/football Text Classification System
=====================================

This repository contains a text classification pipeline designed to categorize posts from the Reddit football community (r/football) into three distinct classes: analysis, criticism, and fact\_dump. The project compares a massive, zero-shot Large Language Model baseline against a locally fine-tuned, lightweight transformer model.

Community Choice and Reasoning
------------------------------

The chosen community is **r/football**, a major hub for global soccer discourse. This community is a highly compelling choice for text classification because online sports discussions are notoriously fluid, sarcastic, and emotionally charged.

Unlike professional journalism, soccer fans frequently blend technical observations with heavy banter, complaints about refereeing, and casual transfer rumors. This creates an intense linguistic overlap where the exact same words (such as names of players, statistics, and tactical terminology) appear across entirely different contexts. Distinguishing between a thoughtful tactical breakdown and an emotional fan rant requires navigating complex sentiment, contextual subtext, and community-specific slang rather than relying on surface-level keyword matching.

Label Taxonomy
--------------

The dataset categorizes text into three mutually exclusive labels:

### 1\. analysis

*   **Definition:** A structured tactical, organizational, or logical argument explaining _how_ or _why_ something happened in football. This includes strategic reviews, historical club performance breakdowns, and system-level evaluations. It can be qualitative opinion if it demonstrates clear structural reasoning.
    
*   **Example 1:** _"Why is Portugal so good at football compared to other nations of similar size? They only have 10 million people but their FA completely overhauled youth infrastructure in the early 2000s, focusing heavily on futsal pipelines for press resistance."_
    
*   **Example 2:** _"What am I missing when it comes to Romelu Lukaku? He is an elite transitional striker who needs space to run into. When managers force him to play with his back to goal in a low-block possession system, his first touch gets exposed."_
    

### 2\. criticism

*   **Definition:** Text where the primary intent is to judge, blame, praise, or vent based on subjective feelings, personal biases, or surface-level match outcomes. This captures the vast majority of standard fan banter, post-match emotional reactions, and complaints about soccer culture.
    
*   **Example 1:** _"The World Cup has been absolutely destroyed this year. Every aspect of it has been overpriced to a ridiculous degree. Tickets are being resold for 800k in Miami... Corporate greed ruined the biggest sporting event in the world."_
    
*   **Example 2:** _"Who is the most overrated player within the past 15 years? For me, it has to be Paul Pogba. Manchester United paid a record fee for him, and he had maybe five world-class games a season. The rest of the time he was walking around lazy."_
    

### 3\. fact\_dump

*   **Definition:** A neutral, factual presentation of football news, transfer rumors, official club or tournament updates, match statistics, or logistical questions without a strong, opinionated core argument.
    
*   **Example 1:** _"Atlético Reject Real Madrid’s €150m Alvarez Bid. Real Madrid have confirmed a €150m offer for Julián Álvarez, but Atlético Madrid rejected it. Atlético insist any move must meet the release clause."_
    
*   **Example 2:** _"LiveScore App and Notifications? Is anyone else having issues with the LiveScore app today? Ever since the latest software update on iOS, my match notifications are either arriving 10 minutes late or not showing up."_
    

Data Collection and Labeling Process
------------------------------------

### Collection Methodology

A total of **200 raw text posts** were collected directly from the r/football desktop browser feed. To ensure the models had rich text to evaluate rather than just single-sentence headlines, searches were filtered using Reddit's self:yes operator to isolate text-heavy discussion threads. Data was gathered across a mix of "New", "Top", and "Hot" sorting queues to capture a diverse sample of breaking news, historical debates, and post-match reactions.

### Labeling and Cleaning

Initial classification drafts were generated using a structured LLM spreadsheet integration to parse out baseline groupings. However, early programmatic passes proved highly unreliable, systematically misclassifying conversational rants containing stats as analysis or stripping out complex bodies.

A thorough, row-by-row manual curation pass was conducted to clean the data. During this human-in-the-loop review, labels were overwritten based on strict developer taxonomy criteria, and formatting artifacts (such as upvote metrics, timestamp text, and UI strings) were stripped out to ensure a clean text block input.

### Label Distribution

The final, curated dataset of 200 rows achieved an incredibly balanced distribution, avoiding any majority-class bias that could compromise training:

### Label Distribution

The final, curated dataset of 200 rows achieved an incredibly balanced distribution, avoiding any majority-class bias that could compromise training:

| Value | Frequency | Percentage |
| :--- | :---: | :---: |
| **criticism** | 74 | 37.0% |
| **analysis** | 69 | 34.5% |
| **fact_dump** | 57 | 28.5% |

### Difficult-to-Label Examples and Annotator Decisions

*   **Example 1: The Multi-Metric Rant**
    
    *   _Text:_ A post furious about the ticket prices and a new 25-minute halftime show for the upcoming World Cup, listing specific financial numbers and routes.
        
    *   _Decision:_ **criticism**. While it included hard data points (like "$800k resale tickets" and "25-minute halftime"), the numbers were weaponized strictly to support an emotional vent calling FIFA leadership "clowns." Because the primary intent was venting rather than an objective economic analysis, it was assigned to criticism.
        
*   **Example 2: The Assist Comparison Thread**
    
    *   _Text:_ A thread stating _"I am tired of people saying assists are the ultimate metric..."_ comparing the career stats of Suarez/Lewandowski against Kroos/Iniesta.
        
    *   _Decision:_ **analysis**. Even though the text kicked off with an angry, typical banter tone (_"I am tired of people..."_), the user went on to construct a highly logical tactical argument defining the technical mechanics of deep midfield playmaking versus penalty-box tap-ins. The structural reasoning overrode the conversational tone.
        
*   **Example 3: Historical Tournament Amnesia**
    
    *   _Text:_ A user asking why the 1978 and 1982 World Cups are rarely talked about online compared to other tournament eras, stating they feel like "middle children."
        
    *   _Decision:_ **criticism**. This post is highly ambiguous because it deals with sports history. However, the author provided zero historical, geopolitical, or structural context inside the text body, relying entirely on a personal, subjective feeling (_"globally I haven't heard anyone talk about him"_). It functioned as a low-effort discussion prompt rather than a factual or analytical breakdown.
        

Fine-Tuning Approach
--------------------

The local model selected for this task was **distilbert-base-uncased**, a highly efficient, lightweight transformer architecture. The pipeline was executed on a free Google Colab **T4 GPU** runtime.

### Training Configuration

The notebook automatically split the 200-row dataset using a standard allocation strategy: **70% Training (140 samples)**, **15% Validation (30 samples)**, and **15% Locked Test Set (30 samples)**.

The fine-tuning run utilized the following hyperparameter framework:

*   **Epochs:** 3
    
*   **Learning Rate:** 2e-5
    
*   **Batch Size:** 16
    
*   **Weight Decay:** 0.01
    

### Hyperparameter Rationale

The choice to stay with a conservative 3-epoch schedule and a 2e-5 learning rate was a deliberate defensive engineering decision. Given the small size of the training pool (140 rows), pushing the epochs higher or expanding the learning rate would run an extreme risk of catastrophic overfitting, causing the model to memorize specific noise or player names unique to the training rows.

Baseline Description
--------------------

To establish a benchmark, a zero-shot LLM baseline was executed on **Llama-3.3-70b-versatile** via the Groq API API. The baseline model evaluated the exact same 30-post locked test set before interacting with any fine-tuning data.

The baseline script was fed a comprehensive system prompt containing explicit plain-language definitions for analysis, criticism, and fact\_dump, along with exactly one pristine, real-world example per label. The model was instructed via explicit formatting guardrails to return _only_ the raw lowercase string of the selected label to guarantee automated evaluation compatibility.

Full Evaluation Report
----------------------

### Model Comparison Metrics

### Model Comparison Metrics

| Evaluation Metric | Zero-Shot Baseline (Llama-3.3-70b) | Fine-Tuned Local Model (DistilBERT) |
| :--- | :---: | :---: |
| **Overall Accuracy** | **70.00%** | **16.67%** |
| **analysis F1-Score** | 0.64 | 0.17 |
| **criticism F1-Score** | 0.78 | 0.11 |
| **fact_dump F1-Score** | 0.67 | 0.22 |
### Fine-Tuned Confusion Matrix

The following matrix represents the text-based directional errors made by the local DistilBERT model on the 30 test examples:

| True \ Predicted | analysis | criticism | fact_dump |
| :--- | :---: | :---: | :---: |
| **analysis** | 2 | 5 | 4 |
| **criticism** | 6 | 1 | 4 |
| **fact_dump** | 5 | 1 | 2 |

### Deep-Dive Failure Analysis

#### 1\. Post #1: The Post-Match Euphoria Thread

*   **Text:** _"Inter 4-3 Barcelona (AET) – What. A. Game. That was absolutely insane. 4–3 after extra time, 7–6 on aggregate. One of the wildest Champions League matches in years..."_
    
*   **True Label:** analysis (Annotated due to systematic aggregate score tracking).
    
*   **Predicted Label:** fact\_dump _(Confidence: 0.34)_
    
*   **Failure Analysis:** The local model fell completely into a numeric keyword trap. Because the text was heavily anchored by numerical entities ("4-3", "7-6", "AET"), DistilBERT automatically mapped these tokens to a sterile fact\_dump news category. It lacked the contextual capacity to recognize that the numbers were being used as an exclamation point for a structural discussion.
    

#### 2\. Post #4: The Scheduling Query

*   **Text:** _"I don't understand the kickoff times? My apologies for being something of a football noob, but I'm wondering if anyone can explain why the matches are starting so late local time? 9pm seems like such..."_
    
*   **True Label:** criticism
    
*   **Predicted Label:** analysis _(Confidence: 0.34)_
    
*   **Failure Analysis:** This post represents a syntax confusion. The user is asking a low-effort question to complain about late television schedules. However, because the text uses an inquisitional framework (_"explain why", "wondering if anyone can"_), the fine-tuned model over-indexed on the interrogative sentence structure, assuming that any text seeking an explanation must be an automated marker for an industrial analysis thread.
    

#### 3\. Post #7: The Transfer Transaction News

*   **Text:** _"Atlético Reject Real Madrid’s €150m Alvarez Bid. Real Madrid have confirmed a €150m offer for Julián Álvarez, but Atlético Madrid rejected it. Atlético insist any move must meet the release clause."_
    
*   **True Label:** fact\_dump
    
*   **Predicted Label:** analysis _(Confidence: 0.34)_
    
*   **Failure Analysis:** This was a pure news flash. The model misclassified it as analysis because terms like "reject", "bid", and "meet the release clause" mimic the structural, conditional vocabulary often found in business or organizational analysis threads. The model snapped onto these transactional vocabulary cues blindly.
    

### Sample Classifications

The table below showcases a direct slice of live classifications generated by the fine-tuned DistilBERT model:

| Post Text Snippet | Predicted Label | Confidence | Status |
| :--- | :---: | :---: | :---: |
| "Are we getting closer to an African team winning the WC? I think the playing field has been levelled this last 2 World cups..." | `criticism` | 0.35 | **Incorrect** |
| "Antoine Griezmann should go down as a legend." | `fact_dump` | 0.35 | **Incorrect** |
| "goalkeepers really do shine at the world cup when the margins are so tight. you get teams that might not have the best outfield players but a world class keeper can keep them in matches..." | `criticism` | 0.34 | **Incorrect** |
| "Leo Messi was the only player to score goals for Argentina from November 2016 till June 2018..." | `fact_dump` | 0.34 | **Correct** |

**Reasonable Correct Prediction Explanation:** While the model recorded an incredibly high error rate overall, a true positive occurred for the Leo Messi stat tracking item. The model was able to successfully resolve this row because the prose was exceptionally short, flat, and completely devoid of exclamation points, conversational slang, or interrogative indicators. This clean style directly matched the baseline statistical co-occurrence of a sterile, objective factual log.diction Explanation:** While the model recorded an incredibly high error rate, a true positive occurred in row #11 for a fact\_dump item where the text simply stated a historic goal record streak. The model was able to successfully resolve this row because the prose was exceptionally short, flat, and completely devoid of exclamation points, conversational slag, or interrogative indicators, matching the baseline statistical co-occurrence of a factual log.
    

Reflection
----------

### Intended vs. Learned Behavior

The ultimate goal of this pipeline was to train a model that could read past the emotional vocabulary of a sports fan and classify text based on the core structural intent of the post. The model was intended to recognize that an author can swear or use slang while still presenting an analytical argument.

In reality, the fine-tuned model failed to capture this nuance entirely. Instead of learning an abstract boundary between analysis and criticism, the model suffered from **representation collapse**. Because the dataset size was so limited (140 training examples), the model’s internal probability distribution flattened out to almost exactly equal shares (~0.34) across all choices. It defaulted to localized token-snapping: if a post had numbers, it guessed fact\_dump; if it had question marks, it guessed analysis. It completely missed the holistic context of the community discourse.

Spec Reflection
---------------

### Specification Guidance

The project specification was an absolute lifesaver regarding the **Label Distribution** guidelines. The spec explicitly warned that if a single class dominated more than 70% of the dataset, training would collapse. This forced a deliberate pivot during data collection to abandon a purely automated Reddit pull and execute a structured manual filter pass, ensuring our final classes sat perfectly balanced at 74, 69, and 57.

### Implementation Divergence

The implementation diverged sharply from the specification's implicit assumption that a fine-tuned model would easily beat or match a generic zero-shot baseline. The specification maps out a classic machine learning narrative where training improves scores.

However, because our specific domain (r/football) relies on intense conversational subtext and highly overlapping vocabulary, our fine-tuned model collapsed to 16.67% while the baseline achieved 70%. The implementation proved that for highly nuanced, low-sample data regimes, a giant model with massive pre-existing world knowledge completely scales past a minor transformer trained from scratch.

AI Usage Section
----------------

### Instance 1: Spreadsheet Data Curation Formulas

*   **Direction:** The AI was directed to create a strict classification formula (=AI()) inside Google Sheets to pre-label Column A into analysis, criticism, or fact\_dump using explicit edge-case definitions targeting "The Tone Trap" and "The Surface Metric Trap."
    
*   **Production:** The AI successfully built and executed the formula string down all 200 rows.
    
*   **Override:** The AI's selections were heavily flawed because the model automatically pushed any post containing standard football arguments straight into criticism whenever it detected basic internet banter words like "trash" or "clown." The initial classifications were completely overwritten using manual Google Sheets filters to preserve true analytical entries.
    

### Instance 2: Error Pattern Identification

*   **Direction:** The 15 text errors generated during the test-set validation cycle were fed back into an LLM with instructions to identify common structural or linguistic themes across the wrong predictions.
    
*   **Production:** The AI surfaced a distinct pattern highlighting the "0.34 Confidence" clustering and flagged how the model was systematically swapping analysis and criticism based entirely on the presence of punctuation hooks like question marks.
    
*   **Override:** The surfaced themes were manually validated by cross-checking the exact rows in the notebook. The automated pattern matching was directly leveraged to construct the technical explanation of the model's representation collapse in the final evaluation write-up.
