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
