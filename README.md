# Project Description

Generate better product titles for a given Amazon ASIN
[Architecture:](https://miro.com/app/board/uXjVMecKdOE=/?share_link_id=772918178966)

[Postman collection:] (https://cloudy-astronaut-885952.postman.co/workspace/Team-Workspace~8c65ae51-ece9-4c8f-8f37-85d4b9f7145e/collection/25444622-9c744239-3e46-4fcc-ac18-fa803719d3b7?action=share&creator=25444622)

Google Cloud Services used:

Secret Manager
Cloud Functions
Cloud Build
Big Query
Natural Language
3rd party services:

Hugging Face ($9/m)
[ScraperApi](https://www.scraperapi.com/)
OpenAI (GPT-3)# Generative Text

1. Start with an Amazon ASIN that we’re going to optimize a Product Title for
2. (GT-2) Extract from its existing text-based listing content all possible keyphrases that describe the product a. Sources to analyze: the listing itself on Amazon (scraping) and what’s pulled into our sp_all_listings_master table in BigQuery for the product b. When scraping, we’re looking to scrape: i. Product Title: id=“productTitle” ii. Key Product Features: id=“feature-bullets” and the a-list-item class underneath iii. Alt text on A+ Content Image: id=“aplus” --> class=“aplus-module” and then img alt underneath that meta description c. In our BQ table, the following fields should have some information: i. item_name = Product Title ii. item_description = Product Decription
3. (GT-3) Based on all keyphrases extracted, we score each one and rank them based on seeming relevance to the original text to develop a shortlist of keyphrases
4. (GT-4) We then take the keyphrases (starting with the most relevant ones) and query any of the ba_search_terms tables in BQ (like sp_ba_search_terms_by_month or sp_ba_search_terms_by_quarter) to see what search terms contain those keyphrases, sorted by SFR in ascending order. Low SFR means more traffic
5. (GT-5) Taking the highly trafficked (low SFR) and calculate their relevancy to the original product listing to see which is the best fit and which is the focal keyword
6. (GT-1) Scrape the Amazon listing’s reviews, put the raw reviews scraped into a table in BQ (not yet created, we need to build one), and extract core concepts from them along with frequency and avg rating
7. (GT-9) Score each review’s concept based on its relevancy to the original listing text
8. (GT-10) Take the focal keyword and search it in Amazon to scrape the top Non-Sponsored ASINs that are our organic SEO competitors using this [URL syntax:] (https://www.amazon.com/s?k=QUERY) for other marketplaces we query the other domains, i.e. for our [UK client] (https://amazon.co.uk/s?k=QUERY)
9. (GT-11) Scrape their listing contents and confirm that they are relevant to our listing
10. (GT-12) For those relevant, scrape the reviews and load them into BQ, then extract their concepts and store them in BQ
11. (GT-18) Relevancy Score: Competitor Review Concepts
12. (GT-17) Aggregate the following to be built into a prompt for GPT a. Low SFR (high volume) highly relevant terms from sp_ba_search_terms. Anything that’s under SFR 1m is considered good volume. We are looking for breadth of keyword descriptors that we can use in our title b.Review concepts from our own reviews that are highly mentioned and highly ranked (avg rating assigned to them) c. Review concepts from other competitor reviews that are highly mentioned and highly ranked and relevant to ours
13. (GT-14) Send the prompt into GPT for different title variations to be output. Maybe take the best 5 out of 25 different generated options

## Zoom Meetings

Zoom meetings:

[1/20/23:] (https://drive.google.com/file/d/1enPFl42ZBVjNu86x_yY2qpJzUY1-QmuZ/view?usp=share_link)
[1/27/23:] (https://drive.google.com/file/d/1XTJ5DKan_VeGL5r6XzU4qAhEVuOn1ch8/view?usp=sharing)
[2/3/23:] (https://drive.google.com/file/d/1Juj7kdcSiXHJ7Nr2UMu4u_JDcpv7MmoT/view?usp=sharing)

# GT14 : Titles and hypotheses Generation

## Overview

This Python script is designed to generate titles and hypotheses for a given product listing based on input data such as search terms, reviews concepts, and competitors' reviews concepts. It uses the FastAPI framework, OpenAI API and Secret Manager. The script defines a FastAPI application and an API endpoint to handle incoming requests and return the generated titles and hypotheses.  

*Test Endpoint : https://gt14-j3sh6qxa4a-uc.a.run.app*  
*Production Endpoint : Not deployed yet*

<!---toc start-->

* [GT14 : Titles and hypotheses Generation](#gt14--titles-and-hypotheses-generation)
  * [Requirements](#requirements)
    * [Python Version](#python-version)
    * [Libraries](#libraries)
    * [External Tools](#external-tools)
  * [How to use it](#how-to-use-it)
    * [Try the deployed API](#try-the-deployed-api)
    * [Running Locally](#running-locally)
    * [Deploying to Cloud Run](#deploying-to-cloud-run)
  * [Functions and Classes](#functions-and-classes)
    * [FastAPI and APIRouter](#fastapi-and-apirouter)
    * [Main Function](#main-function)
      * [Purpose:](#purpose)
      * [Input Parameters:](#input-parameters)
        * [Example:](#example)
      * [Return Values](#return-values)
        * [Example](#example-1)
    * [Helper Functions](#helper-functions)
    * [Schemas](#schemas)
    * [Utils](#utils)
  * [Project Structure](#project-structure)
  * [Testing](#testing)
  * [Contributing](#contributing)

<!---toc end-->

## Requirements

This Python script generates titles and hypotheses using FastAPI, OpenAI API, and Google Secret Manager. Below are the requirements and dependencies needed to run the script.

### Python Version

Python 3.7 or higher

### Libraries
- fastapi==0.68.1
- uvicorn==0.15.0
- gunicorn==20.1.0
- httpx==0.22.0
- pydantic==1.8.2
- google-cloud-secret-manager==2.7.1
- openai==0.27.0

### External Tools

Google Cloud Secret Manager (to store the OpenAI API key)

## How to use it

### Try the deployed API

You can visit the deployed version of the API by adding /docs at the end of the endpoint URL. This will provide access to the API documentation and an interface to try the API.  

Visit the documentation or try the API using this URL : https://gt14-j3sh6qxa4a-uc.a.run.app/docs  

### Running Locally
To run this API locally, follow these steps:  

1. Create and activate a virtual environment.
2. Install the required libraries by running pip install -r requirements.txt.
3. Launch the app using the command python -m uvicorn app:app --reload.
4. Access the API at http://127.0.0.1:8000/.  

Note that you will need to have access to the Secret Manager with your Google Account since the OpenAI API key is stored there. If you do not have access, replace `openai_key` in the file `app/main.py` with your own OpenAI key.

### Deploying to Cloud Run

The API is currently deployed on Cloud Run. To deploy it to your own project, follow these steps:  

1. Login to the project account using the command `gcloud auth login`.
2. Set the project using the command `gcloud config set project <project-id>`.
3. Build the Docker image using the command `docker build -t gcr.io/<project-id>/gt14 .`.
4. Push the image to Google Container Registry using the command `docker push gcr.io/<project-id>/gt14`. You may need to run the command `gcloud auth configure-docker`.
5. Deploy the image to Cloud Run using the command `gcloud run deploy gt14 --image gcr.io/<project-id>/gt14 --platform managed --region us-central1 --allow-unauthenticated`


## Functions and Classes

### FastAPI and APIRouter

`FastAPI()`: Creates a new FastAPI application.  
`APIRouter()`: Creates a new APIRouter instance for handling routes in a modular way.

### Main Function

`generate_title_and_hipothesis(request: Request, asins_input: TitlesInput, number_titles: int = 5) -> TitlesOutput`:  

This function is responsible for generating a list of titles and hypotheses for a given product listing, a list of search terms, reviews concepts, and competitors reviews concepts.

#### Purpose:  

To generate a list of titles and hypotheses using OpenAI API.

#### Input Parameters:

- `asins_input` (TitlesInput):  
    - `search_terms` (List[str]): A list of search terms related to the product.  
    - `reviews_concepts` (List[str]): A list of concepts derived from product reviews.  
    - `competitors_reviews_concepts` (List[str]): A list of concepts derived from competitor's product reviews.  
    - `product_listing` (str): The product listing text.  
- `number_titles` (int, optional): Number of titles to generate. Defaults to 5.  

##### Example:
```json
{
  "asins_input": {
    "search_terms": ["term1", "term2", "term3"],
    "reviews_concepts": ["concept1", "concept2", "concept3"],
    "competitors_reviews_concepts": ["competitor_concept1", "competitor_concept2", "competitor_concept3"],
    "product_listing": "Sample product listing text"
  },
  "number_titles": 5
}
```

#### Return Values

- `titles` (List[str]): A list of generated titles.
- `hypotheses` (List[Dict[str, str]]): A list of dictionaries, each containing a generated title and its corresponding hypothesis.

##### Example

```json
{
  "titles": ["Generated Title 1", "Generated Title 2", "Generated Title 3", "Generated Title 4", "Generated Title 5"],
  "hypotheses": [
    {
      "title": "Generated Title 1",
      "hypothesis": "Generated Hypothesis for Title 1"
    },
    {
      "title": "Generated Title 2",
      "hypothesis": "Generated Hypothesis for Title 2"
    },
    {
      "title": "Generated Title 3",
      "hypothesis": "Generated Hypothesis for Title 3"
    },
    {
      "title": "Generated Title 4",
      "hypothesis": "Generated Hypothesis for Title 4"
    },
    {
      "title": "Generated Title 5",
      "hypothesis": "Generated Hypothesis for Title 5"
    }
  ]
}
```

### Helper Functions

`generate_titles()`: Generates titles using OpenAI API based on the input parameters.  
`generate_hypotheses()`: Generates hypotheses using OpenAI API based on the input titles and product listing.

### Schemas

`TitlesInput`: A Pydantic model defining the input data for generating titles and hypotheses.  
`TitlesOutput`: A Pydantic model defining the output data containing the generated titles and hypotheses.

### Utils

`logger`: A utility for logging messages.  
`get_secret()`: A utility function to get the OpenAI API key from Secret Manager.

## Project Structure 

At the root of the project, you will find the following files :
* `requirements.txt` : lists all the required libraries.
* `README.md` : provides information on how to use the API.
* `main.py` : defines the host and port for FastAPI.
* `Dockerfile` : specifies the Docker configuration.

The `/app` directory contains the following files:
* `schemas`: a folder that contains all the schemas for the API.
* `tests`: a folder that contains all the tests for the API.
* `utils`: a folder that contains scripts such as `logger`, `exceptions` and `secret_manager`.
* `main.py`: a script where all the main logic is defined.
* `openai.py`: a script where all the methods related to the OpenAI API are defined.
* `README.md` : a detailed description of the app, including its purpose, functionality, and any unique features.

## Testing

## Contributing
# GT-4

Test text for gt-4# GT-10

Test text for gt-10# Competitor ASINs Relevancy Endpoint

This endpoint calculates the relevancy scores between a given ASIN and a list of competitor ASINs. The output is a dictionary with the competitor ASINs as keys and their corresponding relevancy scores as values.

## Input

The input to this endpoint is an instance of the `AsinsInput` class , which contains the following fields:

`asin` (string): the ASIN to compare against the competitor ASINs.
`competitor_asins` (list of strings): the list of competitor ASINs to compare against the given ASIN.
`region` (string): the region in which the ASINs are located (e.g. "us", "uk", "de", etc.).

## Output

The output of this endpoint is an instance of the `CompetitorAsinsRelevancyOutput` class, which contains a single field:

`competitor_asins_relevancy` (dictionary): a dictionary with the competitor ASINs as keys and their corresponding relevancy scores as values.

## Description

This endpoint first calls the Listing Helper API for each ASIN in the input to ensure that they are in the database. Then, it retrieves the product data for the given ASIN and each of the competitor ASINs. It then calculates the relevancy score between the given ASIN and each of the competitor ASINs using OpenAI's `get_listings_relevancy_openai()` function. Finally, it returns a dictionary with the competitor ASINs as keys and their corresponding relevancy scores as values.

If there is an error while calling the OpenAI API, the endpoint will return a 500 error with a message indicating the error.

## Example

### Input

```json
{
  "asin": "B07H65KP63",
  "competitor_asins": ["B07H65KP63", "B07H65KP63", "B07H65KP63"],
  "region": "us"
}
```

### Output

```json
{
  "competitor_asins_relevancy": {
    "B07H65KP63": 1.0
  }
}
```
# GT-26

Test text for gt-26