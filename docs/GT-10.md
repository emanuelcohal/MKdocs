# Focal Keyword Search (GT-10)

This Python script searches Amazon for given search keywords and extracts the ASINs of non-sponsored products from the first 4 pages of search results. The script uses the Scraper API to scrape the data and BeautifulSoup library to parse the HTML content.

## Current endpoint: 

gt-10-focal-keyword-search: https://gt-10-focal-keyword-search-kknzgohiuq-uc.a.run.app

## Description

The script contains a main function that takes search keywords and an optional Amazon region as input.  
It then searches Amazon for the given keywords and extracts the ASINs of non-sponsored products from the first 4 pages of search results.  
The script returns the list of extracted ASINs along with the search keywords.

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

## Dependencies

BeautifulSoup: To parse the HTML content.  
Scraper API: To scrape the data from Amazon.  
Logging: To log the information.  

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

### Local setup
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
