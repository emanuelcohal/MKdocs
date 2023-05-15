# Listings Relevancy (GT-11)

## Overview

This script is a relevancy checker that uses OpenAI's GPT-3.5-turbo to determine the relevancy between two product listings on a scale of 0 to 1 (0 being totally irrelevant and 1 being most relevant). It takes client and competitors ASINs (Amazon Standard Identification Number) as input and returns the relevancy score between them.

## Table of Contents

<!---toc start-->

<!---toc end-->

## Current endpoint: 

*gt-11-listings relevancy: https://gt-11-listings-relevancy-kknzgohiuq-uc.a.run.app*

## Requirements

To run the script, the following requirements should be met:

- Python environment with the necessary libraries installed (`os`, `functions_framework`, `bs4`, `scraper_api`, `logging`,`openai`).
- The `OPENAI_KEY` environment variable should be set with a valid API key for the OpenAI service.
- The script should be deployed and configured to handle HTTP requests.

## Flow Description

The script receives an HTTP request with the required inputs (`our_product_asin` and `relevant_products_asin`).
It calls the `load_listing_data` function for each ASIN to load the product data from an external API.
The `load_listing_data_bq` function is called to load the product data from a BigQuery table.
The `get_listings_relevancy_openai` function is called to determine the relevancy score between the product listings using OpenAI's GPT-3.5-turbo.
The final result is returned as a JSON object containing the relevancy scores for each ASIN in the `relevant_products_asin` list.
### Input

The input is a JSON object containing two keys:

`our_product_asin`: The ASIN of the product we want to compare (e.g., B0874XN4D8).
`relevant_products_asin`: A list of ASINs of the relevant products (e.g., ['B09VLK9W3S', 'B09VSR2ZHD']).

Example input:

```json
{
  "our_product_asin": "B0874XN4D8",
  "relevant_products_asin": ["B09VLK9W3S", "B09VSR2ZHD"]
}
```

### Output

The output is a JSON object containing the relevancy scores for each ASIN in the relevant_products_asin list. The keys are the ASINs, and the values are the relevancy scores.

Example output:

```json
{
  "results": {
    "B09VLK9W3S": 0.8,
    "B09VSR2ZHD": 0.6
  },
  "our_product_asin": "B0874XN4D8"
}
```

In this example, the relevancy score between our product (B0874XN4D8) and B09VLK9W3S is 0.8, and the relevancy score between our product (B0874XN4D8) and B09VSR2ZHD is 0.6.

## How to use it

### To deploy this locally:

To deploy this locally:

1. Create a Python virtual environment (Python 3.11.1) and activate it
    ```bash
    python -m venv venv
    source venv/bin/activate
    ```
2. Activate the virtual environment using `source venv/bin/activate`

3. Install the requirements file
    ```bash
        pip install -r requirements-dev.txt
    ```

4. Run the development server using `functions-framework --target main --debug --port 8080`
5. Access the app home page at `http://127.0.0.1:8080/`
