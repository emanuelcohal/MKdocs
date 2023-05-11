# GT14 : Titles and hypotheses Generation

## Overview

This Python script is designed to generate titles and hypotheses for a given product listing based on input data such as search terms, reviews concepts, and competitors' reviews concepts. It uses the FastAPI framework, OpenAI API and Secret Manager. The script defines a FastAPI application and an API endpoint to handle incoming requests and return the generated titles and hypotheses.  

Test Endpoint : https://gt14-j3sh6qxa4a-uc.a.run.app  
Production Endpoint : Not deployed yet

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

`request (Request)`: The request object.  
`asins_input (TitlesInput)`: The input data containing product listing, search terms, reviews concepts, and competitors reviews concepts.  
`number_titles (int, optional)`: Number of titles to generate. Defaults to 5.

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

`asins_input` (TitlesInput):  
    `search_terms` (List[str]): A list of search terms related to the product.  
    `reviews_concepts` (List[str]): A list of concepts derived from product reviews.  
    `competitors_reviews_concepts` (List[str]): A list of concepts derived from competitor's product reviews.  
    `product_listing` (str): The product listing text.  
- `number_titles` (int, optional): Number of titles to generate. Defaults to 5.  

#### Return Values

`TitlesOutput`: An object containing the titles and hypotheses generated.

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

- `titles` (List[str]): A list of generated titles.
- `hypotheses` (List[Dict[str, str]]): A list of dictionaries, each containing a generated title and its corresponding hypothesis.

### Helper Functions

`generate_titles()`: Generates titles using OpenAI API based on the input parameters.  
`generate_hypotheses()`: Generates hypotheses using OpenAI API based on the input titles and product listing.

### Schemas

`TitlesInput`: A Pydantic model defining the input data for generating titles and hypotheses.  
`TitlesOutput`: A Pydantic model defining the output data containing the generated titles and hypotheses.

### Utils

`logger`: A utility for logging messages.  
`get_secret()`: A utility function to get the OpenAI API key from Secret Manager.

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

# Testing

# Contributing
