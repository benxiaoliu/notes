It’s the name of the feature, and some statistics about it. These statistics help us make sure that the header is aligned with the feature, since that is hard to track with all the features we have. This also allows us to know what features to debug if the values are varying significantly between steps.

We generate these feature statistics at every stage in the PW OFE pipeline when something happens to the data. So the purpose of the website is to automatically detect if/when significant changes occur to the statistics of the features.

Build one web site to visualize data and read test data from local disk, for demo purpose.
Test data as attached and visualization as below
Use multiple select component to allow user to select multiple data to display
Inject the same test data into database and hook up web site with database to visualize it
The only difference is ready data from database, instead of local disk
Read test data from cosmos and inject into database, need one button to force data sync up
This need cosmos access which you will need to access corpnet for develop purpose
Build real data in cosmos and visualize real data from web site
@Chuck Wang will need to upload all debug statistics to cosmos and integrate with final experience

I just discussed with Qingwei offline about some of the specifics for the website. You should start with Qingwei’s scenario first, which is visualizing the comparison between a single value from pipelines. These values will be the RMSE and data volume (you can also break them down into top markets), from pipelines. This will allow us to automatically compare results from the different pipelines when we add new techniques to ensure no regression occurs.

 

The next step will be to allow some comparison with multiple values from pipelines, such as the top 20 features from each pipeline, or allowing the searching to see which pipelines have a specific features. We do not have to worry about comparing the feature statistics that I mentioned before yet.
