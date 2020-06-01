# JobAd Enrichments API 
 
## Getting Started 
 
***“What do employers actually want from a job seeker?”***
Jobad Enrichments API extracts relevant labor market data from job ad texts, making it possible to automatically see what the employers need or request from the job seekers.   
This enables intelligent job matching, labor market analysis, and provides up-to-date job market terminology in a synonym dictionary   
  
 
## Table of Contents 
* [What the API does](#What the API does) 
* [Example of use case when using the API](#Example of use case when using the API) 
* [The main endpoints of the API](#The main endpoints of the API) 
* [Start using the API (request your API key to get going today)](#Start using the API) 
  
## What the API does 
 
- Extracts found terms in a Swedish job ad text for competences, soft 
  skills, job titles and place of work and automatically predicts if 
  they are requested by the employer or not.   In other words, the API 
  makes it possible to extract important labor market data from raw and 
  unstructured job ad texts. 
- Returns the found job titles that the job seeker is expected to work 
  as    
- Returns the found competences/skills that the employer requests 
  from the job seeker    
- Returns the found soft skills/traits that the 
  employer requests from the job seeker    
- Returns the found places of    work (city names, districts, country 
  names, names of subway stations    and shuttle stations)       
- Handles misspelled terms        
- Handles singular/plural terms       
- Handles extraction of compound words, for example ‘hälsovård’ in 
  ‘hälso- och sjukvård’ 
- Provides a synonym dictionary with job titles, competences, traits and places of work. The same synonym dictionary is used in the API for term extraction    and translation between synonyms. The dictionary is built with a bottom up approach, and only contains labor market terms that have been found in actual job ads. 
 
## Example of use case when using the API 
 
Makes it possible to improve search results when used in a job search application/engine since it targets the requested competences/skills/job titles from job ads. When knowing if the terms are requested or not, low quality free text search results can be avoided. 
  
The functionality in the API has been used in [Platsbanken](https://arbetsformedlingen.se/platsbanken/) since May 2019 to improve the quality of the search results.   
   
 
## The main endpoints of the API 
 
Our API and documentation can be found at [Swagger-UI](https://jobad-enrichments-api.jobtechdev.se/)   

Brief description of the endpoints:   
 
**/enrichtextdocuments** 
returns all identified terms in the ad and with a prediction for the term, a decimal between 0.0-1.0 how likely it is   
that the term is requested by the employer. The closer to 1.0 = Requested by the employer, the closer to 0.0 = Not requested by the employer.    
**/enrichtextdocumentsbinary** Returns only the requested terms when the prediction exceeds a certain classification threshold.   
If a job title or a geographical term (place of work) is mentioned in the job ad headline, the job ad will always be enriched with these terms regardless of the result of the automatic predictions.   
**/synonymdictionary** Returns data and terms for the synonym dictionary that is used in the API when extracting known labor market terms.    
Makes it possible to connect synonym terms found in the job ad text with synonym terms found in the user search input in a job ad search solution.   
*Example:* The job ads that are searchable in Platsbanken are continuously enriched through the JobAd Enrichments API with concept terms, for example the concept term ‘sjuksköterska’ (‘nurse’) for job ads containing the job title in singular form or plural form or as a misspelled job title.   
The end user in Platsbanken writes ‘sjuksköterskor’ (‘nurses’) as a search term, which translates to the searchable concept term ‘sjuksköterska’ in the search query.    
Results are returned for job ads where the employer seeks a ‘sjuksköterska’ (singular), ‘sjuksköterskor’ (plural) or ‘sjukssköterska’ (misspelled). 
 
## Start using the API
To start using the API today, request your own free API key at [https://apirequest.jobtechdev.se/](https://apirequest.jobtechdev.se/) 
When you have retrieved your API key, you can try it out in [Swagger-UI](https://jobad-enrichments-api.jobtechdev.se/) 
For authentication, your API key must be sent as a http header parameter in every API request.
 
