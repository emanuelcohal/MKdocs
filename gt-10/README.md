# Focal Keyword Search (GT-10)

## Overview

This Python script searches Amazon for given search keywords and extracts the ASINs of non-sponsored products from the first 4 pages of search results. The script uses the Scraper API to scrape the data and BeautifulSoup library to parse the HTML content.

## Table of Contents
<!---toc start-->

* [Focal Keyword Search (GT-10)](#focal-keyword-search-gt-10)
  * [Current endpoint:](#current-endpoint)
  * [Requirements](#requirements)
    * [Flow Description](#flow-description)
    * [Input](#input)
    * [Output](#output)
  * [Example](#example)
    * [Input](#input-1)
    * [Output](#output-1)
  * [Amazon Regions](#amazon-regions)
  * [How to use it](#how-to-use-it)
    * [To deploy this locally:](#to-deploy-this-locally)

<!---toc end-->

## Current endpoint: 

*gt-10-focal-keyword-search: https://gt-10-focal-keyword-search-kknzgohiuq-uc.a.run.app*

## Requirements

To run the script, the following requirements should be met:

- Python environment with the necessary libraries installed (`os`, `functions_framework`, `bs4`, `scraper_api`, `logging`).
- The `SCRAPER_API_KEY` environment variable should be set with a valid API key for the Scraper API service.
- The script should be deployed and configured to handle HTTP requests.

### Flow Description

The main function takes search keywords and Amazon region as input.  
It validates the input and calls the `search_amazon()` function with the search keywords and region.  
The `search_amazon()` function searches Amazon for the given keywords and extracts the ASINs of non-sponsored products from the first 4 pages of search results.  
The `parse_data()` function is used to parse the HTML content and extract the ASINs.  
The list of extracted ASINs is returned along with the search keywords.

### Input

search_keywords: A list of search terms to search on Amazon.  
region: (Optional) Amazon country site to search. Default is 'us'.  

### Output

scraped_asins: A list of extracted ASINs.  
search_keywords: The original list of search keywords.  

## Example

### Input

```json
{
  "search_keywords": ["laptop", "smartphone"],
  "region": "us"
}
```

### Output

```json
{
  "scraped_asins": [
    "B08N5WRWNW",
    "B08N5LNQCX",
    "B08N5M7S6K",
    "B08N5M8Z4K",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3",
    "B08N5LFLC3"
  ],
  "search_keywords": ["laptop", "smartphone"]
}
```

## Amazon Regions

The script supports the following Amazon regions:

- us: amazon.com
- uk: amazon.co.uk
- ca: amazon.ca
- de: amazon.de
- es: amazon.es
- fr: amazon.fr
- it: amazon.it
- jp: amazon.co.jp
- in: amazon.in
- cn: amazon.cn
- sg: amazon.com.sg
- mx: amazon.com.mx
- ae: amazon.ae
- br: amazon.com.br
- nl: amazon.nl
- au: amazon.com.au
- tr: amazon.com.tr
- sa: amazon.sa
- se: amazon.se
- pl: amazon.pl


## How to use it

### To deploy this locally:

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
