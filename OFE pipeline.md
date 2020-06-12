Online Feature Extraction pipeline

OFE: Bing online feature extraction data, where we get features to train with
Deep dive: here
https://www.bingwiki.com/wiki/OFE_Data_Process_Pipeline
https://www.bingwiki.com/wiki/OFE_Pipeline_Developers
https://www.bingwiki.com/wiki/OFE_Ranking_Experiments
Sync up latest OFE pipeline code:
https://www.bingwiki.com/wiki/Relevance_Git_Ramp_Up
Key scope script:
                        i.      https://msasg.visualstudio.com/Bing_and_IPG/_git/RelevanceFundamentals/?path=%2Fprivate%2FOFE%2FScopeScripts%2FMergeLogs%2FMerge.script&version=GBmaster

Sangam deployment:
                        i.      https://microsoft.sharepoint.com/teams/CoreDataPlatform/SitePages/Home.aspx

                      ii.      \\lsstc146\searchgold\deploy\builds\data\Sangam_Partners\Sangam-Prod1\RankFun_OFE\OFEEnv

To get this, you need to sync SearchGold, how?
SearchGold: data depot where Bing online service consume: Wiki: here
What’s scope?
Cosmos: big data process platform
                        i.      Wiki: https://microsoft.sharepoint.com/teams/Cosmos/SitePages/Home.aspx

                      ii.      Relevance VC: relevance VC is our VC to run jobs

https://cosmos09.osdinfra.net/cosmos/relevance/
Wiki: here
                    iii.      Scope: language build on cosmos

Wiki: https://microsoft.sharepoint.com/teams/Cosmos/SitePages/Scope.aspx
ScopeStudio: VS plugin for Scope job development
ScopeTutorial: learn scope language step by step
















Model training:
Training features: features which used by online model and dump it to log for training feature collection
                                                   i.      So this can get huge amount of training data by free

Training target: based on user click behavior, like whether it get clicked, how long user stay on clicked page
Model: lambda rank model (decision tree based)
Evaluation set: similar as training data generation
Cosmos extraction is another way for 1.a
Offline extraction: Crawl bing.com with query and log the features for the crawl request
Offline extraction features, which is time consuming and not scalable
Tail queries -> query frequency is low, like < 5
SSRx -> Session Success Rate -> measure online user behavior
SBS -> Side By Side -> Serp 1 vs. Serp 2, which is better
Serp -> Search Engine Results Page



When submitting a scraping job with specific ranker augumentation on TechHub, corresponding scraping result will be stored on Cosmos. Cosmos is a big-data platform, which storing and dealing with big data. However, we don’t know where is our scraping job results exactly. What I need to do is to get the 15 features by dealing with QuerySelection log, QueryResult log and IFM log on cosmos using scope language. In details, select TraceID, QueryID from QuerySelection log, get DocID by joining QuerySelection log and QueryResult log using TraceId and QueryId. At last, get features by selecting IFM log using DocId and TraceId = CacheTraceId, QueryId = CacheQueryId.

Please add below into query augmentation, this is used to enable OFE logging. Use this as filter in Script.
[tla:forceofequery:20][urp:orcaofeplan:\data\URP\Prod\OFEExecutionPlan_Passage_20190207.ini][urp:option:Default|RankingCacheableFetcher|EnableOFEExtractionRanker|true][urp:option:Default|RankingCacheableFetcher|OFEExtractionRankerName|\data\tlaranking\ryapolin\Fusion\UR.PW.Extraction.Passthrough.Top.Iter4_NoTM.Reduced.ini][urp:option:FastBrain:Recall|RankingCacheableFetcher|EnableOFEExtractionRanker|true][urp:option:FastBrain:Recall|RankingCacheableFetcher|OFEExtractionRankerName|\data\tlaranking\ryapolin\Fusion\UR.PW.Extraction.Passthrough.Top.Iter4_NoTM.Reduced.ini]


 help with some data analysis for the OFE fetch issue as part of my ramp up project.

The background is that when we fetch the OFE data, sometimes the results are missing in the OFE logs. An example would be the query “facebook” missing the top url result of “facebook.com”, but the lower positions are in the data are present. So in the log we see a no url at position 1, but urls at positions 2-10 are there.

We’re trying to get some statistics on the missing positions, since top missing positions will likely have a bigger impact. Additionally, we need the top Q-U pairs to know which pairs have the biggest impact. These are some initial tasks you can get started with:

Can you create graphs per market that shows the position distribution of the data for these 16 markets?

Can you get the # impressions for each <market,query,url> tuple so we know the most popular query-url pairs by market?
 

I’ve extracted all of the missing urls with positions (Rank column) from xSlapi here:
https://cosmos09.osdinfra.net/cosmos/relevance/projects/metrics/charwan/DataDebug/allfix/Missing.ss?property=info



Tasks conclusion:

 

Get SelectionLog, ResultLog, OFELog containing 100 TraceIds.
AugQuery of the 100 TraceIds should contain augmentation “OFEExtractionRankerName”
Dictionary initialization : Write a C# function in the script to add the 100 traceId to the script as a dictionary
Dump all rowSets in SelectionLog, which contains TraceId in the dictionary
Dump all rowSets in ResultLog, which contains TraceId in the dictionary
Dump all rowSets in OFELog, which contains TraceId in the dictionary
 

 

 

Analyze data of one traceId.
AugQuery of the TraceId should contain augmentation “OFEExtractionRankerName”
Dictionary initialization : Write a C# function in the script to add the  traceId to the script as a dictionary (reuse codes of task 1)
Dump all rowSets in SelectionLog, which contains TraceId in the dictionary
Dump all rowSets in ResultLog, which contains TraceId in the dictionary
Dump all rowSets in OFELog, which contains TraceId in the dictionary
Analyze whether one TraceId has several DocId and one DocId has several featureList, as well as how many features a featureList has.

Below is the extraction to get the whole line. You can create 2 functions in C#:

 

string GetTraceIdFromLine(string line)
bool IsTraceIdExist(string traceId)
 

Then, you can OUTPUT “line” which traceId exist. Thanks.

 

A =

    SELECT line

FROM @QuerySelectionPath

USING DefaultTextExtractor("-d","\n");



Kensho Alert:
Backend data stats: https://kensho2/#/datafeed/785cc4d6-bc04-4add-94ad-87a9f0d76458

Output impression stats: https://kensho2/#/datafeed/92dee5a0-eacb-4e7d-a8c0-c2206a713c08

Feature log stats: https://kensho2/#/datafeed/7a74a713-4a22-4a30-aa5b-8c9d41e3acc6

The charts are generated from stats data here: https://cosmos09.osdinfra.net/cosmos/relevance/projects/OnlineFeatureExtraction/SummaryStatisticsV2/

Stats generating script is just Merge.script.

 

Could you go over the existing series/data sources? Maybe start from kensho2 wiki. If you have more specific questions, we can discuss then.




Now looks like we’re having too many unnecessary series in FeatureLog stats, which could be charged. It should be an easy task to remove most of them, from data generation pipeline.

review Benxiao’s script for OFE verification?

 

 uploading codes to Azure DevOps, After code review, we should checked in the code.
 
 Please use the following augmentations to verify the OFE extraction ranker.

[urp:option:Default|RankingCacheableFetcher|EnableOFEExtractionRanker|true][urp:option:Default|RankingCacheableFetcher|OFEExtractionRankerName|ranker path+filename]

[urp:option:FastBrain:Recall|RankingCacheableFetcher|EnableOFEExtractionRanker|true][urp:option:FastBrain:Recall|RankingCacheableFetcher|OFEExtractionRankerName|ranker path+filename]

Get one test extraction ranker INI
\ data\tlaranking\ryapolin\Fusion\UR.PW.Extraction.Passthrough.Top.Iter4_NoTM.Reduced.ini
Setup extraction ranker in below augmentation
Confirm with Ye whether the augmentation is correct
Do TH scraping job to log the OFE features in new way
https://techhub.ms/app/v2/TechniqueList/scope=Mine
Check IFM log (L2 log) whether the L2 features in new extraction ranker are logged correctly
Check merge.script to see how to parse IFM log

 Benxiao generated L2 features using TH: https://techhub.ms/app/v2/ExperimentDetail/experimentId=bbe8baeb-9012-49f9-82b3-b4019a6d004c,scope=Parameters,dependencyRootId=39034bd1-a31e-4af2-bab0-c87d520feff1,stage=Scraping

 

She found that the L2 features are still 1000+, instead of 15 features we specified in extraction ranker.

 

Below is one example and Benxiao will provide more examples tomorrow. Would you please help to check the gap? Thanks!

 

https://cosmos09.osdinfra.net/cosmos/relevance/local/users/v-benliu/test/TraceAugFeatures.txt?property=info


Based on corrected path, the L2 features count looks expected, distribution as below.

 

The only abnormal thing is that, some documents get only 1 feature, which is “StaticRank”. For further investigation:

 

Dump out 2 traceid with selection log / results log / IFM log, as attached. @Ye Tian (BING), would you please help to check what’s the gap?
Check production IFM log, to check feature count distribution, to see if 1 feature still high
 

