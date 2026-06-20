COMMUNITY

I choose r/football because I have played football my whole life, I have the domain knowledge and understand all the information that is inside the community, thus the most logical choice was to pick this as the community. There is also a great deal of different perspectives from all around the globe, football is a creative sport and is not solved by any means so there are very interesting data to be found in the space while also being structured enough to work for a classification task.

LABELS

- analysis: The post makes a structured tactical argument, organizational or statistical breakdown explaining HOW or WHY something happened or is happening on the pitch or financially or logistically. Soccer is a game that does not rely purely on statistics, therefore, a decently thought out opinion without any numbers to back it up still counts as analysis. 
    - Playing at the Azteca isn't just playing Mexico it's playing Mexico, 80,000 screaming fans, AND the environment. Extreme heat days in Mexico City have surged from 2 days a year to 11 days annually and the stadium is completely open air. Then there's the altitude the Azteca sits at 7,350 feet, which drains aerobic capacity fast.You might feel fine in warmup. By the 70th minute you're running on empty. Foreign teams simply don't prepare for that combination of heat, altitude and atmosphere. Mexico live in it. They train in it. It's basically a home superpower. The Azteca alone could carry them through the group stage.
    - I know it's often called one of the greatest finals ever, but was it really? For about 80 minutes, it felt completely one sided. Argentina were dominating and France looked lost, barely creating anything and getting outplayed all over the pitch.

Did the last 40 minutes (including extra time and penalties) make people forget how one-sided most of the match actually was, or am I missing something?

- fact_dump: A neutral, factual presentation of news, transfer rumors, statistics tables, personal experiences or injury reports without an analytical argument or strong opinion. Even if facts appear as criticism or make a certain entity look bad. 
    - After Argentina lost to Saudi Arabia in their opening game, I genuinely thought Messi’s last World Cup was about to end in disaster.
    That night, in peak football fan delusion, I made a deal with God.
    I said, “Please let Messi win the World Cup. If Argentina somehow goes all the way and lifts the trophy, I’ll shave my head bald.”
    Fast forward a month later, Argentina beat France in one of the greatest finals ever, Messi finally completed football, and suddenly I remembered the promise I’d made.
    A few days later, I was sitting at the barber’s shop watching my hair fall to the ground.
    No regrets.
    What’s the craziest promise, superstition, or ritual you’ve done because of the FIFA World Cup?
    - Omar Abdulkadir Artan, a FIFA-selected World Cup referee and CAF’s 2025 African Referee of the Year, was reportedly denied entry to the United States on arrival and sent back to Turkey. No official reason has yet been given for the decision.

- criticism: The post's primary intent is to judge, blame, praise, or vent about a player, manager, or board decision based on subjective feelings or surface-level outcomes. 
    - These "Hydration breaks" are the biggest middle fingers to all fans.
    - neymar wasn’t fit for the wc but they still finessed a spot for him. no one is calling him greedy and self centered because it’s not ronaldo, joao pedro is at home doing fine btw.


HARD EDGE CASE

    The post:
        A lot of chatter this world cup around compositions of teams from what is expected with what is reality. The defense says "they were born in the nation"... "or they were naturalized" or "they are attached to roots". or "this is the new world"... Ok fine. We all agree.

        But then why don't you have problem with FIFA's allocation of WC spots by region which is the core problem where talent doesn't get opportunity.

        The current unfair allocation has to be the most obvious exampled of dreaded r word.

        Confederation (Region)	Total Countries (FIFA Members)	Direct Slots (2026)	Regional Success Rate (%)
        UEFA (Europe)	55	16	29.09%
        CAF (Africa)	54	9	16.67%
        AFC(Asia)	45	7	15.56%
        CONMEBOL (South America)	10	6	60.00%
        CONCACAF (North & Central America)	35	6	17.14%
        OFC(Ocenia)	12	2	16.67%
        Total / Overall	211	46*	21.80%
        For people not good with stats, the above table implies ASIA and AFRICA got the worst deal. If we go by population, it will be far far worse and unfair.

        Everyone shall reflect how wrong FIFA is.
        Why does LATAM have 6 spots for 10 nations while Asia has same count for 45 nations?

        Curacao has 0/26 who are natives. Since 26 Dutch decided to switch flags so that they get to play WorldCup. Now this is unfair to Asian/African countries who have so limited slots. While thanks to above colonial era mindset of FIFA, NA/SA have ample spots that even such teams get free ticket to FIFA. All they need to do is turn up while African/Asian teams need to grind against 50 for 4-6 spots

        The actual fair allocation ne below inclusive of host nations:

        Africa: 12 seats for 54 nations

        Asia: 10 seats for 45 nations

        Europe and all those islands colonies: 13 seats for 55 nations

        North America, South America, Aus/NZ/Oceania and everybody else: 13 seats of remaining 57 nations

        If one stands against the critics of compositions of squad, one should also stand against the unfair representation. How come the "business reasons" become suddenly more important than representation?

        No way you are telling me that a Ecuadorian deserves representation more than a Thai.
    
    The Ambiguity: The post uses highly charged, accusatory, and subjective framing ("colonial era mindset", "dreaded r word"), which strongly signals criticism. However, it relies on an explicit table of FIFA member-to-slot ratios and builds a structured alternative allocation model.

    The Final Label: analysis

    The Decision Rule applied: Tone is a secondary feature; structural intent is primary. Because the user constructs a verifiable mathematical argument using hard ratios to justify their position, it must be labeled analysis, regardless of how aggressive or emotional the prose is.

DATA COLLECTION PLAN

Data will be sourced entirely from the r/football subreddit. To ensure a robust dataset, I will not pull sequentially from a single feed. Instead:

    50% of the data will be scraped from the "Hot" and "New" feeds to capture raw, everyday discourse (heavy on criticism and info_dump).

    50% of the data will be pulled from the "Top" feed to intentionally hunt for long-form, high-effort content (heavy on analysis). Will also search for key soccer technical terms to find quality analysis posts more easily.

Target sample size: A minimum of 200 total examples, aiming for roughly 60–70 examples per class to maintain an even balance

EVALUATION METRICS

If our dataset is 33% criticism and the model simply guesses criticism every single time, it achieves 33% accuracy while being completely useless. Therefore we need per class accuracy for each label so correct predictions ÷ total episodes in test set, for this specific case I will use per class F1-Score.

DEFINITION OF SUCCESS

    [Random Guessing Baseline: ~33.3% Macro F1] 
            │
            ▼
    [Minimum Acceptable Threshold: Macro F1 ≥ 65%] - this means the model must successfully classify roughly two out of every three unseen posts.
            │
            ▼
    [Ideal Goal: Macro F1 ≥ 80%]

    The fine-tuned DistilBERT model must achieve an Overall Accuracy that is at least 5% higher than the zero-shot Llama-3.3 baseline

AI Tool Plan

Instruct it to generate 10 highly ambiguous mock posts that sit directly on the boundaries (such as statistical rants with heavy sarcasm, or breaking news carrying aggressive subjective framing).

I will use Gemini 3.5 Flash-Extended to act as an algorithmic pre-annotator for the posts so I can accelerate the labelling process.

Post-evaluation, all test set examples misclassified by the fine-tuned DistilBERT model will be isolated and passed into an LLM with the prompt (after giving it the necessary context): "Analyze these misclassifications from our fine-tuned classifier. Identify 3 systematic linguistic features, lengths, or contextual patterns that could be confusing the model."