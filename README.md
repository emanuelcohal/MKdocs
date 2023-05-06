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

# GT-2

Extract from its  existing text-based listing content all possible keyphrases that describe the product a. Sources to analyze: the listing itself on Amazon (scraping) and what’s pulled into our sp_all_listings_master table in BigQuery for the product b. When scraping, we’re looking to scrape: i. Product Title: id=“productTitle” ii. Key Product Features: id=“feature-bullets” and the a-list-item class underneath iii. Alt text on A+ Content Image: id=“aplus” --> class=“aplus-module” and then img alt underneath that meta description c. In our BQ table, the following fields should have some information: i. item_name = Product Title ii. item_description = Product Decription
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