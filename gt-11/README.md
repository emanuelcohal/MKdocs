# Competitor ASINs Relevancy Endpoint

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
