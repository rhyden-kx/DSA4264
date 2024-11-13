# DSA4264 Technical Report:   
Project: Reddit Analysis  
Members: Caleb Lee Heng Yi, Denise Teh Kai Xin, Lin Zhengjue Elisa, Neleh Tok Ying Yun, Sarah Goh Yue En     
Last updated on 13/11/2024

---

# 1. Context
As a widely used social platform, Reddit hosts a vast diversity of discussions, and subreddits like _[r/Singapore](https://www.reddit.com/r/singapore/)_ , _[r/SingaporeRaw](https://www.reddit.com/r/SingaporeRaw/)_, and _[r/SingaporeHappenings](https://www.reddit.com/r/singaporehappenings/)_ serve as critical spaces for Singaporeans to discuss local social, political, and current affairs. While these communities provide avenues for constructive conversation, they have also become arenas for toxic language and hate speech, often targeting minority groups or appearing in heated political discussions. Research has shown that unchecked toxicity and hatefulness can exacerbate divisions within communities and diminish the quality of public discourse ([Rieger et al., 2021](https://doi.org/10.1177/20563051211052906); [Kumar et al., 2023](https://doi.org/10.1145/3543507.3583522)). These challenges are intensified by Reddit's limitations in moderating localized language patterns, especially as users employ Singlish and code-switching that can evade conventional detection.
The default Reddit moderation tools fall short in effectively filtering content that contains these nuanced expressions. This has left Reddit moderation insufficient to meet the unique needs of these Singaporean communities. The result is an environment where harmful content can thrive unchecked, impacting both community members and the broader online discourse.

Our project aims to fill this gap by leveraging LionGuard, a Singapore-specific language model that interprets local nuances in toxic and hateful language. However, we still go further: our project undertakes a comprehensive analysis of toxicity and hatefulness patterns, detecting which discussions or events trigger the most intense language. This research is set to provide unprecedented insight into localised toxic behaviour on Reddit, revealing critical areas where moderation could be improved to better serve Singaporean users.

Through this research, we will deliver data-driven recommendations to strengthen content moderation practices. Our work will not only guide policies for safer Reddit communities but will also help lay the groundwork for advanced, culturally aware moderation tools that could be adapted for other multilingual or multicultural settings. By focusing on these goals, our project seeks to contribute meaningfully to online safety and trust, fostering an online space that better reflects the values of respectful discourse and inclusivity. 

---

# 2. Scope
## 2.1 Problem
Our main stakeholder, the Ministry of Digital Development and Information’s (MDDI) Online Trust and Safety department, specifically the team focusing on online hate speech and toxicity, faces the problem of regulating hatefulness and toxicity on Reddit. The intensity of hateful and toxic comments on Singapore’s subreddits has been steadily increasing in the past 3 years, and more sharply in the recent months. With online hatefulness and toxicity becoming more widespread, the racial and religious harmony in Singapore’s diverse society is threatened. Children and youth, who are more likely to be affected and influenced by toxic and hateful content, may have their safety and well-being compromised too. This will prove harder for the MDDI team to ensure a peaceful and safe digital space for Singaporeans.

If hateful and toxic sentiments in Singapore subreddit spaces are not curbed, it may continue to increase at a higher rate, which will make the subreddits even harder to regulate. As online spaces tend to be echo chambers, hateful and toxic sentiments will be reinforced and multiplied, exacerbating the problem ([Cinelli et al., 2021](https://www.pnas.org/doi/10.1073/pnas.2023301118)). Hence, it is crucial to address this problem as soon as possible.

The amount of data from Singapore subreddit comments is too large to process manually. Data science and machine learning will be useful in many ways. Firstly, a good large language model (LLM) is able to measure the hatefulness and toxicity of text consistently, and provide accurate metrics of hatefulness and toxicity levels across time. Secondly, data science can dig deeper into the attributes of hateful and toxic comments to deduce patterns and reasons driving these sentiments. This would enable the creation of more effective solutions that target the root causes of increasing hatefulness and toxicity.

## 2.2 Success Criteria
We aim to achieve a reduction of 5% in hatefulness and 36% in toxicity levels upon implementing our recommendations. These goals are based on the sharp increase in these metrics since April 2023, compared to the overall time frame, and our objective is to bring these levels back to a more manageable rate.

Additionally, by introducing a more automated moderation process, we aim to improve operational efficiency, significantly reducing the need for manual moderation efforts and the associated time and costs. This increased efficiency will enable MDDI to better allocate resources and respond swiftly to emerging issues. Moreover, the project will enhance moderation precision by providing a tool specifically calibrated for Singaporean discourse. This precision will reduce false positives in toxicity detection, fostering greater trust among users and ensuring that moderation accurately reflects the cultural nuances of local discussions. In the event where there would be another spike in toxicity and hatefulness levels occurring in the future, the root cause can then be effectively addressed.

Ultimately, our success will be measured not only by the reduction in toxic and hateful content but also by the effectiveness of an automated, culturally-sensitive moderation tool that MDDI can deploy to maintain a safe and harmonious online space for Singaporeans.

## 2.3 Assumptions
The following are the assumptions for our project:

1. Sufficient manpower to review toxic/hateful comments that are flagged out by moderation bot.
2. Toxicity and hatefulness within subreddits do not spread outside itself. This allows us to take the difference of the 2 percentage changes to give us our success metric numbers.
3. The current state of Reddit stays the same, i.e. no addition of new features to Reddit. Addition of new features will require us to re-analyse how the addition of the new feature affects the problem.

---

# 3. Methodology
## 3.1 Experimental Design
Our project hinges on 2 main algorithms, LionGuard for sentiment analysis scoring, and LDAMallet for topic modelling. We also utilised other tools such as Word Clouds, matplotlib graphs to visualise our processes and results. ![Fig. 1](https://github.com/rhyden-kx/DSA4264/blob/main/images/Methogology%20fllowchart.png) 
<p align="center"> Fig. 1 </p>

### 3.1.1 LionGuard:
[LionGuard](https://arxiv.org/html/2407.10995v1) is a pretrained sentiment scoring model developed by Govtech for Singaporean contexts, which does both embedding as well as classification. It combines the Beijing Academy of AI (BAAI) General Embedding (BGE) with a Ridge Classifier to produce its Binary Classifier and Multi Label classifier. The Binary Classifier detects if a document is Safe or Unsafe (0 or 1), while the Multi Label scores each document across 7 categories: hateful, harassment, public harm, self-harm, sexual, toxic, violent; ranging (-∞, +∞), where higher scores indicate more positive class prediction. Each model for the Multi-Label was fine tuned to suit that context. 

As the data we are exploring comes from Singaporean subreddits, it tends to skew towards local vernacular. Thus we selected this model as it was trained on data from the Singaporean forum Eat Drink Man Woman and also other reddit datasets. 

Evaluation of Lionguard by its developers showed a better PR-AUC score compared to other general LLMs, such as HateBERT and SingBERT, as well as XGboost and Neural Network classifiers. Considering our end goal is to establish safe online consumerism, using the PR-AUC score is a good combination of Precision and Recall, especially for imbalanced classes where positive classes are rare. 

### 3.1.2 LDAMallet:
LDAMallet comes from the [MALLET](https://mimno.github.io/Mallet/topics) Java-based package, and uses MALLET’s sampling-based implementation of Latent Dirichlet Allocation (LDA). We used this model over others due to its ability to seed topics, which we required due to the nature of our analysis and hypotheses. We also felt that Latent Semantic Analysis would be less useful considering the short and efficient communication style of Singaporeans and lack of contextual vocabulary within comments (compared to original posts).

We seeded the model across 9 distinct topics: Political, Covid-19, Race and Religion, Transport, Relationships, Crims, Housing, Education and Work. If the model failed to classify a document, it was marked as Unknown. These topics came from our initial exploratory topic modelling, which used LDA as well.

We assessed the seeded model using the [Gensim CoherenceModel](https://palmetto.demos.dice-research.org/), since LDAMallet is under the Gensim library. We tested it across 3 coherence scoring models: c_v, and c_uci u_mass, yielding scores: 0.7213, -0.172, -5.589 respectively. The c_v score indicates a relatively acceptable level, but the c_uci and u_mass score indicates slightly weaker coherence.


## 3.2 Limitations
1. **Model Limitations in Toxicity and Hatefulness Differentiation:** LionGuard’s toxicity and hatefulness scores are limited by a lack of category-specific insights, especially in detecting less explicit, context-dependent hatefulness markers. The model’s bias toward explicitly harmful language may mean it under-reports subtle, situational hatefulness often present in complex discussions around race or religion.  
2. **Limitations in Topic Modelling accuracy due to lack of meaningful corpus:** We acknowledge that the topic modelling coherence scores in this analysis is weak, this is due to the method of Coherence Scorings which take into account term frequency and n-grams probability. Due to the short nature of comments, as well as unconventional sentence structures of Singlish, it would affect the effectiveness of coherence scoring systems.
3. **Limitations in Topic Model Precision Due to Resource Constraints:** The project employed LDA-based topic modelling constrained by limited computational resources. Consequently, we focused on broad themes without exploring more granular subtopics, which may restrict the specificity of our conclusions, especially for nuanced or high-volume discussions. This constraint limits the granularity achievable in our findings, which could impact the depth of subtopic insights for large-scale discussions.

## 3.3 Technical Assumptions
1. **Model Suitability for Localized Content:** We assume that the LionGuard model, designed specifically for Singaporean discourse, is suitable for detecting the nuanced toxicity and hatefulness in local contexts, especially compared to global models like HateBERT. This is based on LionGuard’s localized training data, which captures both English and Singlish language patterns, making it more attuned to local language subtleties.
2. **Sampling Representativeness:** Due to computational constraints, our analysis uses a sample-based approach, with the assumption that the randomly selected 500,000 comments provide an accurate representation of the overall discourse in each subreddit and across time. We assume these samples encapsulate the typical range of toxicity and hatefulness levels present, despite inevitable content exclusions.
3. **Data Structure and Labeling Reliability:** We assume that LionGuard’s labels for toxicity and hatefulness—calibrated for Singaporean language nuances—are accurate and contextually relevant to Singaporean Reddit discussions. This means that LionGuard’s categorizations reliably differentiate between levels of toxicity and hatefulness based on linguistic and cultural markers. This assumption is central to our analysis, as we rely on LionGuard’s labels to consistently interpret toxicity and hatefulness scores across various local topics, situated in Singaporean internet culture.
4. **Threshold Sensitivity for Cultural Nuances:** We assume that LionGuard has an effective threshold for detecting culturally specific terms and local discourse patterns. This means we expect LionGuard to accurately classify varying levels of toxicity and hatefulness in Singlish or informal language commonly seen in Singaporean discourse without conflating colloquial expressions with harmful intent.

## 3.4 Data
1. The Data used for this project was provided in class by Professor Khoo. It is a collection of 4932728 rows of reddit comments from 2020 to 2023, with additional data such as timestamp, moderation information and subreddit IDs. 
2. Data Filtering and Pre-Processing Constraints: We removed any non-english characters, punctuation and emojis from the text comments to aid in language processing for our sentiment analysis. We note that Singaporean English relies heavily on expressive language markers, including emoji use and non-standard terms. Consequently, certain comment tones or intents may have been diluted or misinterpreted due to these preprocessing decisions, potentially skewing our analysis.
3. In our initial preprocessing we also removed any empty comments, as well as comments marked as [deleted] or [removed] since they would not be useful for sentiment analysis. However, we later found out that some of these removals were due to moderation, and reprocessed the data to analyse these moderated comments.
4. Unaddressed Singaporean Linguistic Features: We removed stopwords using the NLTK English stopword list. However, Singaporean stopwords (e.g., "lah," "lor," "ah") were not included, which could introduce noise in our topic models and word frequency analyses. The absence of these localized stopwords may have skewed outputs, possibly leading to noise in our topic model outputs and misrepresentations in frequency-based visualizations like word clouds, subsequently affecting the clarity of our insights.
5. Data Representation of Subreddit-Specific Dynamics: Each subreddit analyzed in this study—*r/Singapore*, *r/SingaporeRaw*, and *r/SingaporeHappenings*—caters to distinct user groups and content moderation philosophies. This diversity provides valuable insights into various community dynamics but also complicates the generalization of findings. For instance, while *r/SingaporeRaw* allows for more unrestricted speech, *r/Singapore* may adopt a stricter approach to content moderation. These variances impact the scalability and applicability of our recommendations across communities, given the different operational cultures.

--- 
# 4. Findings:
## 4.1 Trend over time
### Hypothesis 1: Toxicity & hatefulness has been increasing overtime. ✅*True*

![Fig. 2](https://github.com/rhyden-kx/DSA4264/blob/main/images/daily_average_hatefulness_and_toxic_scores.png)  
<p align="center"> Fig. 2 </p>

By plotting the daily average hateful and toxic scores , we can see that toxicity and hatefulness has increased over time. Both of which saw a sharp increase since March 2023, but especially the months of September and October that year.

It can also be observed that there is more variation in the toxic scores than in the hateful scores.

By averaging by month, we calculate that hatefulness and toxicity have seen an 8% and 58% increase over time respectively.

![Fig. 3](https://github.com/rhyden-kx/DSA4264/blob/main/images/number_of_comments_across_time.png)  
<p align="center"> Fig. 3 </p>

This plot shows that the number of comments has largely hovered at 200 to 500 per day , with exception of a few spikes. Thus, comments are generally getting more toxic in their content.

We proceeded to investigate what factors were contributing to the sharp increase in hateful and toxic scores since March 2023.

## 4.2 Trend Across Subreddits
### Hypothesis 2: Toxicity and hatefulness varies across subreddits. ✅ *True*

![Fig. 4](https://github.com/rhyden-kx/DSA4264/blob/main/images/subreddit%20toxicity.png)
<p align="center"> Fig. 4 </p>

We plotted the toxicity scores across subreddits , and saw that most of the comments in *r/SingaporeRaw* and *r/SingaporeHappenings* are more toxic than in *r/Singapore*.

![Fig. 5](https://github.com/rhyden-kx/DSA4264/blob/main/images/monthly_ave_toxic_score_by_subreddit.png)
<p align="center"> Fig. 5 </p>

We plot the average toxic scores per month , and saw all 3 subreddits increase in average toxicity. However, we noticed that *r/SingaporeHappenings* only started to have average toxic scores since September 2022.

![Fig. 6](https://github.com/rhyden-kx/DSA4264/blob/main/images/monthly_comment_count_by_subreddit.png)
<p align="center"> Fig. 6 </p>

We investigate why this was the case and discovered it was because comments in *r/SingaporeHappenings* only existed since September 2022 . Thus, we concluded that it was because of the subreddits that resulted in the large increase in toxicity around May 2023.

We sought to explain the overall increase in toxicity and hatefulness across the entire timespan.

## 4.3 Trend Across Topics
### Hypothesis 3: Major One-off Events (eg. GE, affair, iswaran, israel-hamas; COVID: Lockdown) are more toxic and hateful than median every day comments. ❌ *False*

Next, we want to investigate whether major one-off events correlated with more toxic scores. 

First, we went to find days that recorded the most number of comments. We found 3 days. From the 1st plot, we found 2 days : 2020-07-10, which was polling day, and 2021-12-07, which was the day news broke out that COVID restrictions were extended. From the 2nd plot, we found 2023-07-17, which coincided with the day news broke out that MP Tan Chuan Jin and Cheng Li Hui had an affair.  
![Fig. 3 (repeated)](https://github.com/rhyden-kx/DSA4264/blob/main/images/number_of_comments_across_time.png)
![Fig. 7](https://github.com/rhyden-kx/DSA4264/blob/main/images/monthly_comments_by_year.png)
<p align="center"> Fig. 7 & 8 </p>


Next, we searched for the day with the median average toxicity score and compared it with the previous 3 days we found earlier.
 
![Fig. 8](https://github.com/rhyden-kx/DSA4264/blob/main/images/hateful_and_toxic_one_off_events.png)
<p align="center"> Fig. 9 </p>

All days recorded similar toxicity and hatefulness scores . Thus, we conclude that major one-off events do not influence toxicity and hatefulness scores.


### Hypothesis 4: Certain topics would be more toxic/hateful. ❌*False*

For this hypothesis we investigated the toxic and hateful scores for the 10 topics we got from our topic modelling. However, we found that there were similar toxicity and hatefulness scores across all topics.

![Fig. 9](https://github.com/rhyden-kx/DSA4264/blob/main/images/toxic_and_hatefulness_by_topic.png)
<p align="center"> Fig. 10 </p>


We also decided to investigate the distribution for the common content categories of the top 20 (excluding daily discussion posts) most active posts based on the number of comments. We grouped the top 20 post titles into 6 content categories based on title name, not to be confused with topics. Comparing the distributions of toxicity scores of each content category, we yielded results that align with, reinforcing that the accuracy of our topic modelling was not the cause behind the lack of trend.

![Fig. 10](https://github.com/rhyden-kx/DSA4264/blob/main/images/toxic_by_post_content_category.png)
![Fig. 11](https://github.com/rhyden-kx/DSA4264/blob/main/images/hateful_by_post_content_category.png)
<p align="center"> Fig. 11 </p>

Thus we concluded that toxicity and hatefulness do not vary significantly for particular topics. 

However, we noticed that generally toxicity scores are higher than hatefulness scores. For instance, in both figures all of the 10 topics and 6 content categories had 75th percentiles that were not considered hateful (score < 0) but were considered toxic (score > 0) respectively. 

![Fig. 12a](https://github.com/rhyden-kx/DSA4264/blob/main/images/toxic_content_category_wordcloud.png) 
<p align="center"> Fig. 12a </p>

![Fig. 12b](https://github.com/rhyden-kx/DSA4264/blob/main/images/hateful_content_category_wordcloud.png) 
<p align="center"> Fig. 12b </p>


Fig 12a and Fig 12b shows the wordclouds of most frequent words for comments that were determined to be toxic (score > 0) and hateful (score > 0) respectively. We observe that all 6 content categories had toxic comments but only GE2020 had hateful content. 

![Fig. 13a](https://github.com/rhyden-kx/DSA4264/blob/main/images/overall_toxic_content_category_wordcloud.png) 
<p align="center"> Fig. 13a </p>

Looking at Fig, 13a, the frequent words from the overall toxic comments of the top 20 posts align with Lionguard’s definition of toxic. The model detected rude, disrespectful, or profane comments. 

![Fig. 13b](https://github.com/rhyden-kx/DSA4264/blob/main/images/overall_hateful_content_category_wordcloud.png) 
<p align="center"> Fig. 13b </p>

While Lionguard is meant to detect hatefulness in terms of race, gender, ethnicity, religion, nationality, sexual orientation, disability status, or caste. The hateful comments were mainly related to race and gender, showing that these areas contributed most to the hatefulness, as seen in Fig. 13b. 

## 4.4 Trend of Moderation
We decided to use whether the text column in our dataset contained the string ‘[removed]’ or not as a gauge for measuring the moderation of comments by moderators. This is different from the moderation column in the original dataset given to us.
The rationale for this choice is threefold.

First, there are more entries with ‘[removed]’ as its text than in the moderation column, thus the former gives us a more accurate number of actual removed comments. 

Second, the variables in the moderation list represent more automatic moderation actions, e.g. collapsed comments which are collapsed based on the number of upvotes and downvotes it receives.

Third, the variables in the moderation list are not consistent throughout our data, both between different subreddits and within subreddits. This is because different subreddits have different moderation actions. Therefore, using the text column is more consistent to compare moderation activity across different subreddits.

Lastly, [research](https://www.reddit.com/r/help/comments/91ni5k/comment/e2zdg08/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) has shown that '[removed]' was due to moderator activity either by the mods or the auto-moderation rules of the subreddit.

### Hypothesis 5: There is not enough moderation on days with higher toxicity. ✅*True*

![Fig. 14](https://github.com/rhyden-kx/DSA4264/blob/main/images/ave_toxic_and_manual_removed_across_time.png)
<p align="center"> Fig. 14 </p>

In general, toxicity has increased, while moderation activity has varied, but has largely remained the same over time. There are 3 peaks in average toxic score: one in July 2020, May 2022, and May 2023 onwards, especially after August 2023. All 3 periods show an increase in toxicity in comments, but a dip or constant number of removed comments, during those periods.

Thus, we conclude that an increase in toxicity in general has not been met with an increase in moderation.

We pick moderation in months rather than days here, to get a bigger picture of moderation across a larger timeframe, rather than restricting it on a day-by-day basis.

## Hypothesis 6: Toxic/hateful users and subreddits are not moderated enough. ✅ True

![Fig. 15](https://github.com/rhyden-kx/DSA4264/blob/main/images/monthly_count_removed_comments_by_subreddit.png)
<p align="center"> Fig. 15 </p>

Moderation between the 3 subreddits differ . This shows that moderation is correlated with toxicity of the subreddits, seen earlier in Fig. 5.

We expect the plot to look more like a straight line passing through the origin, because more toxic users should receive more moderation.

![Fig. 16](https://github.com/rhyden-kx/DSA4264/blob/main/images/ave_toxic_removed_comments_by_user.png)
<p align="center"> Fig. 16 </p>

But we get a scatterplot that is quite flat. We observe that there is a good number of users who have high average toxicity scores, but have no comments being removed. This means that toxic users are not sufficiently moderated.

From the plot, we also see that users posting comments in *r/SingaporeRaw* and *r/SingaporeHappenings* tend to be on the right side of the plot, meaning that users found in these 2 subreddits are more toxic than users found in *r/Singapore*.

Overall, both toxic users and subreddits can be more moderated.

--- 
# 5. Discussion
Based on our results from subsection “Trend Overtime” we validated that toxicity and hatefulness on these 3 subreddits was indeed increasing overtime. We decided to zoom in on toxicity for this section as we found from the subsection “Trend Across Topics” that the severity of toxicity was significantly more than hatefulness. In conclusion we came up with 2 theories to explain the rise of toxicity on these subreddits overtime.

### 5.1.1 Theory 1: Limited moderation tools due to Reddit’s API Pricing changes
Firstly, for *r/Singapore*, its average toxicity increases even though the number of comments have decreased over time. This shows that the comments are becoming more toxic in content. From the subsection “Trend of Moderation” we observed the sudden and continuous spike in the average toxic score around May 2023 coincides with the drop in moderation activity for *r/Singapore*. Some desktop research alluded that during that time period reddit made changes to their API pricing that negatively affected moderators.  

On April 19, 2023 Reddit announced that access to its [API will no longer be free](https://www.reddit.com/r/reddit/comments/12qwagm/an_update_regarding_reddits_api/?rdt=42111). Effective June 19, 2023, their updated Data API Terms will introduce a paid premium access point for third parties who require additional capabilities, higher usage limits, and broader usage rights. Reddit’s explanation for this change was that much of its data was now being used to train Artificial Intelligence. 

This announcement was met with [intense backlash](https://www.theguardian.com/technology/2023/dec/30/reddit-moderator-protest-communities-social-media#:~:text=Reddit%20executives%20reasoned%20that%20the%20changes%20were%20needed%20to%20prevent%20companies%2C%20especially%20artificial%20intelligence%20startups%20creating%20large%20language%20models%2C%20from%20using%20Reddit%E2%80%99s%20data%20for%20free) from Reddit’s unpaid content moderators and users with a key concern being the loss of many third-party apps and moderation tools unable to foot the high cost. On June 12, 2023, over 8,000 subreddits, including *r/Singapore* participated in a 48hr blackout period, temporarily closing their subreddit in protests. Moderators on *r/Singapore* echoed the sentiment that [many moderators use third party moderator tools](https://www.reddit.com/r/singapore/comments/142if79/on_june_12_rsingapore_will_be_going_dark_in/).

However, [CNBC](https://www.cnbc.com/2023/06/16/reddit-in-crisis-as-prominent-moderators-protest-api-price-increase.html#:~:text=Although%20Reddit%20has,as%20outside%20services) found that though Reddit claims that selected moderation tools will remain unaffected, moderators were sceptical. Moderators claimed that historically Reddit has made promises to introduce high quality internal moderation tools that ended up falling below expectations.

Additionally, *r/Singapore* moderators have mentioned that production of their own moderation tool bot has been affected due to this new policy. This new tool would supposedly result in quicker and more effective moderation. However, since plans were disrupted, their moderation processes would have been largely affected. We can see this effect in how moderation dips in Quarter 3 of 2023, which coincides with the implementation of the new API prices on July 1st.

Thus, we theorise that due to Reddit’s API pricing changes the moderation capacity on subreddits such as *r/Singapore* has dropped due to the now limited access to moderation tools. 

### 5.1.2 Theory 2: Rising popularity of inherently toxic subreddits
We found from the subsection “Trend Across Subreddits” that *r/SingaporeRaw* and *r/SingaporeHappenings* have a higher average toxic score compared to *r/Singapore*. Additionally, these 2 subreddits are growing in popularity in recent years, based on their increasing number of comments. 

Upon further investigation we found that these 2 subreddits have either no community rules or significantly fewer rules compared to *r/Singapore* . In fact the subreddit description of *r/SingaporeRaw* seems to be promoting itself as an uncensored subreddit where “Redditporeans” can feel safe to post freely without being silenced. Thus, these 2 subreddits are by their nature meant to provide an alternative space to the more restrictive *r/Singapore* for redditors who want a less moderated experience.
![Fig. 17](https://github.com/rhyden-kx/DSA4264/blob/main/images/subreddit_rules.png)
<p align="center"> Fig. 17 </p>

 
This de-emphasising of censorship ties into looser moderation, leading to more toxic and hateful posts and comments being allowed on to the subreddit. This then leads to more users being encouraged to join in and post increasingly toxic and hateful posts and comments, which in turn do not get moderated. Hence, this cycle perpetuates and amplifies toxicity and hatefulness 
![Fig. 18](https://github.com/rhyden-kx/DSA4264/blob/main/images/ToxicCycle.png).
<p align="center"> Fig. 18 </p>


## 5.2 Fairness:
Analysing Reddit data for Singapore subreddits presents a challenge in ensuring fair representation due to the uneven user base sizes, which affects the distribution of comments in our sampled dataset. With *r/Singapore*'s 1.5 million users vastly outnumbering the 78k users in *r/SingaporeRaw* and 43k in *r/SingaporeHappenings*, our findings and topic modelling are naturally more influenced by *r/Singapore*'s discussions . This imbalance means that prominent trends in *r/Singapore* may overshadow unique or less common themes from the smaller subreddits. While *r/Singapore*'s larger dataset offers broad insights, it limits our ability to fully capture a balanced view of discourse across all three communities. Given our time constraints, we had to work within this limitation.

![Fig. 19](https://github.com/rhyden-kx/DSA4264/blob/main/images/subreddit_comment_count.png)
<p align="center"> Fig. 19 </p>


## 5.3 Deployability:
We have developed a jupyter notebook which allows users to run our workflow and analysis methods to generate results for larger scales of data. We have chosen a range of methods and visualisations which are most relevant for trend analysis, and yet simple for non-technical users to understand. There are also dynamic visualisations, which allow the user to select date ranges for more intricate analyses.

We note however, that the LDAMallet method for topic modelling is deprecated, and not supported in later versions of the Gensim package. Thus while we have included it, and directions on how to run it, we have also included another method for running using the Little Wrapper method which is still maintained.

## 5.4.  Recommendations
## 5.4.1 Current situation - Singapore:
Currently, students in Singapore have their own Personal Learning Devices (PLDs) which have [softwares to restrict their content](https://www.straitstimes.com/singapore/new-app-to-manage-students-chromebooks-out-by-november-2024-ipads-by-january-2025) (MobileGuardian, Lightspeed Systems, Jamf, or Blocksi). However, [this is limited](https://www.reddit.com/r/SGExams/comments/14ojm28/hate_mobile_guardian/) in its abilities to properly restrict NSFW and toxic content from those devices. Students have mentioned that it is overly restrictive, blocking important applications such as Whatsapp, and also has many loopholes which allow them to access NSFW content regardless. Furthermore, they are likely to have their own personal devices which are not school-issued that can access toxic and hateful content regardless.

We would aim to tackle the root problem, which is to curb the creation of spaces for toxic content. However, as [Article 14(1)](https://sso.agc.gov.sg/Act/CONS1963?ProvIds=pr14-) of the Constitution of the Republic of Singapore guarantees to Singapore citizens the rights to freedom of speech and expression, peaceful assembly without arms, and association, it would not be legal, nor ethical to just remove these spaces entirely. Furthermore, direct interference would be highly unpopular amongst the public, especially in subreddits such as *r/Singaporeraw* where the aim is for unfiltered speech. 

### 5.4.2 Current Situation - Reddit:
Reddit’s current automatic moderation filters are not reliable and are inadequate. The filter does not take into consideration the semantic meaning of the full sentence, but bases it off individual words. As seen below, derogatory terms such as “CECA” (a term typically used derogatorily to describe migrant workers from India under a specific work scheme), were not flagged as hateful ![Fig. 20](https://github.com/rhyden-kx/DSA4264/blob/main/images/Reddit%20filter.png).
<p align="center"> Fig. 20 </p>


This leads to an issue of moderators needing to create their own rules and processes to flag out negative content, which relies on manpower and is highly subjective to the individual moderator. As mentioned by moderators in *r/Singapore*, they are [unable to develop better moderation tools](https://www.reddit.com/r/singapore/comments/148iou9/blackout_reflection_how_has_rsingapore_going_dark/) to aid in their work due to the new API call changes. Thus our solution would aim to reduce the proportion of toxic content, and also help improve current Reddit moderation capabilities.

### 5.4.3 Recommendation 1: Moderation Guard
For spaces such as *r/Singapore*, which are generally not toxic or hateful, we could develop a moderation bot powered by Lionguard (or a similar LLM) calibrated for Singaporean vocabulary. It would feature a flagging system to vet every post and comment in that subreddit for toxic and hateful comments, and mark them accordingly. Moderators can adjust the threshold for an acceptable level of hatefulness and toxicity, and those which are above that threshold will be flagged for moderators to check. Additionally, it would also flag out to moderators users with a high toxic or hateful activity score who join the subreddit.
 
Deployment would be in batches, starting with subreddits more keen on toxicity moderation such as *r/Singapore* and *r/SGExams* (a subreddit for students). These subreddits currently moderate for hateful content, and would be the most willing to adopt this tool. We could then expand to other subreddits, and negotiate with their moderators on an acceptable threshold of toxicity and hatefulness. We would also work with Reddit to bypass the API rate limit which is currently in place, since this tool would be considered as useful for moderation. We would push for collaborating with the owners and moderators of these online platforms to implement more rules and moderating tools, especially in places which cater to wider age ranges that includes children.


### 5.4.3 Recommendation 2: Community Outreach
Our second recommendation would be more community outreach. Since the goal is for a safe online experience for children in general, another solution to consider is public outreach, where we educate parents and children on safe internet usage. We would encourage parents to restrict their children’s access to spaces with such negative content, through means such as Parental Controls methods from [Google](https://safety.google/intl/en_sg/families/parental-supervision/) and [Apple](https://support.apple.com/en-sg/105121). These functions allow parents to manage their child’s screen time, and app downloads. 

Children should receive greater education on safe internet practices to promote better online etiquette and reduce toxicity within their communities. Many schools already provide ICT (Information and Communication Technology) classes to teach essential technological skills, so expanding this curriculum to include responsible technology use could offer meaningful benefits. Emphasis would be placed on accountability of their online personas, and that they should aim to use the internet for good, rather than to spread negativity.

# 6. Conclusion
While toxicity and hatefulness has been increasing in recent months, we have found it can be attributed to the echochamber effect of spiralling toxicity and hatefulness cycles. Additionally, lack of adequate moderation also perpetuates the existence of such content on the web. Thus, it would be in the best interest of the future generations if actions are taken to reduce children's exposure to these online places. 

