# Stream API for job ads - getting started

The aim of this text is to walk you through what you're seeing in the [Swagger-UI](https://jobstream.api.jobtechdev.se) to help you download all the job ads being published to Arbetsförmedlingen. This is much easier than using the search API to download all content. The jobstream API is best used indexing the ads by their id in your backend.

# Table of Contents
* [Authentication](#Authentication)
* [Endpoints](#Endpoints)
* [Results](#Results)
* [Errors](#Errors)



## Short introduction

The endpoints for the ads stream API are:

* [Stream](#Stream) - returning all active ads that have been updated after a given moment, or between two timestamps.

* [Snapshot](#Snapshot) - returning all active ads.

The easiest way to try out the API is to go to the [swagger page](https://jobstream.api.jobtechdev.se/).
But first you need a key to authenticate yourself.

## Authentication
For this API, you will need to register to get your own API key at [apirequest.jobtechdev.se](https://apirequest.jobtechdev.se)

## Endpoints
Below we only show the URI's. If you prefer the curl command, you type it like:

	curl "{URL}" -H "accept: application/json" -H "api-key: {proper_key}"
	
### Stream 
/stream

The stream endpoint gives you all the changes to the dataset of ads that are currently open for application. This means 3 types of events that can appear, new ads, removals of ads and updates of ads. By using the stream endpoint you will be able to quickly update your data set with small and efficient calls and responses. 
	
You are required to give a timestamp that determines the starting time from when you want your ads. For example sending a time from yesterday at half past two will give you all changes between that timestamp and now. The format for the timestamp is YYYY-MM-DDTHH:MM:SS The rate limit is one request per minute. An organisation that wants to keep up a realtime copy of all the ads from Arbetsformedlingen would have their app doing a call like this once every minute: 

    	/stream?date=2020-09-24T10:00:00

Where the time given is always now() minus a minute.
Alternatively for more exactness, you can specify a date range to get all ad changes (that are currently open for application) within that range: 
    
    	/stream?date=2020-05-03T10:00:00&updated-before-date=2020-05-04T10:00:00
	
If you want to filter ads for a subset of the job market you can use occupation_ids as filters. This means you can filter your results for geographical areas: country, region (län), municipality (kommun). You can also filter using concept_ids for occupation_field, occupation_group and occupation_name according to the JobTech Taxonomy.

    	/stream?date=2020-09-22T13:11:06&occupation-concept-id=MVqp_eS8_kDZ&location-concept-id=oDpK_oZ2_WYt
	
Both the geographical and occupational filters are hierarchical so the highest level you are filtering for is what you will get. If you give several of the same level you will get the ads corresponding to all of those.


### Snapshot
/snapshot

The snapshot endpoint will give you the job ads that are currently open for application. Without the specific events for removals or updates like in the stream endpoint. No parameters are needed. Output file is about 300 Mb, not recommended for use in a browser.
	
### Code examples
Code examples for accessing the api can be found in the 'getting-started-code-examples' repository on Github: 
https://github.com/JobtechSwe/getting-started-code-examples


	
If you’re looking for more advanced search options, please check our [JobSearch API](https://jobtechdev.se/docs/apis/jobsearch/).

## Results
The results of your queries will be in [JSON](https://en.wikipedia.org/wiki/JSON) format. We won't attempt to explain the ad objects attribute by attribute in this document. Instead we've decided to try to include this in the data model which you can find in our [Swagger GUI](https://jobsearch.api.jobtechdev.se) for JobSearch.

Successful queries will have a response code of 200 and give you a result set that consists of the ad events that happened within the timespan you set. 

These events can be of 3 different kinds: New ads, updated ads, and removed ads. New and updated will look the same, the only thing that distinguishes them from each other is that an updated ad has an ID that's already in the database of open ads. 

A removal object looks like this:

	{
	    "id": 8460292,
	    "removed": true,
	    "removed_date": "2020-01-13T13:03:26"
	    "occupation": "eU1q_zvL_9Rx",
	    "occupation_group": "sq3p_WVv_Fjd",
	    "occupation_field": "Gazf_2TU_kJw",
	    "municipality": "cUyN_CsV_HLU",
	    "region": "9hXe_F4g_eT4",
	    "country": "i46j_HaG_va4"
	  }

These are typically grouped together in your result set so if you're request has a larger timespan than a few minutes you may have to scroll to se actual job ads.

## Errors
Unsuccessful queries will have a response code of:

| HTTP Status code | Reason | Explanation |
| ------------- | ------------- | -------------|
| 400 | Bad Request | Something wrong in the query |
| 401 | Unauthorized | You are not using a valid API key |
| 429 | Rate limit exceeded | You requested too much during too short time |
| 500 | Internal Server Error | Something wrong on the server side |

