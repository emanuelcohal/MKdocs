# GT14 : Titles and hypotheses Generation

# Overview

This Python script is designed to generate titles and hypotheses for a given product listing based on input data such as search terms, reviews concepts, and competitors' reviews concepts. It uses the FastAPI framework, OpenAI API and Secret Manager. The script defines a FastAPI application and an API endpoint to handle incoming requests and return the generated titles and hypotheses.

# Requirements

This Python script generates titles and hypotheses using FastAPI, OpenAI API, and Google Secret Manager. Below are the requirements and dependencies needed to run the script.

## Python Version

Python 3.7 or higher

## Libraries
- fastapi==0.68.1
- uvicorn==0.15.0
- gunicorn==20.1.0
- httpx==0.22.0
- pydantic==1.8.2
- google-cloud-secret-manager==2.7.1
- openai==0.27.0

## External Tools

Google Cloud Secret Manager (to store the OpenAI API key)

## Functions and Classes

### FastAPI and APIRouter

`FastAPI()`: Creates a new FastAPI application.
`APIRouter()`: Creates a new APIRouter instance for handling routes in a modular way.

### Main Function

`generate_title_and_hipothesis(request: Request, asins_input: TitlesInput, number_titles: int = 5) -> TitlesOutput`:  
This function is responsible for generating a list of titles and hypotheses for a given product listing, a list of search terms, reviews concepts, and competitors reviews concepts.

**Purpose**:  

To generate a list of titles and hypotheses using OpenAI API.

**Input Parameters**:

`request (Request)`: The request object.
`asins_input (TitlesInput)`: The input data containing product listing, search terms, reviews concepts, and competitors reviews concepts.
`number_titles (int, optional)`: Number of titles to generate. Defaults to 5.

**Return Values**:

`TitlesOutput`: An object containing the titles and hypotheses generated.

### Helper Functions

`generate_titles()`: Generates titles using OpenAI API based on the input parameters.
`generate_hypotheses()`: Generates hypotheses using OpenAI API based on the input titles and product listing.

### Schemas

`TitlesInput`: A Pydantic model defining the input data for generating titles and hypotheses.
`TitlesOutput`: A Pydantic model defining the output data containing the generated titles and hypotheses.

### Utils

`logger`: A utility for logging messages.
`get_secret()`: A utility function to get the OpenAI API key from Secret Manager.

# Testing

# Contributing
