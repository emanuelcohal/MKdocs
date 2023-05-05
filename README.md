# Main Readme file
<!-- add-file: ./gt-2/gt-2.md --># GT-2

Extract from its  existing text-based listing content all possible keyphrases that describe the product a. Sources to analyze: the listing itself on Amazon (scraping) and what’s pulled into our sp_all_listings_master table in BigQuery for the product b. When scraping, we’re looking to scrape: i. Product Title: id=“productTitle” ii. Key Product Features: id=“feature-bullets” and the a-list-item class underneath iii. Alt text on A+ Content Image: id=“aplus” --> class=“aplus-module” and then img alt underneath that meta description c. In our BQ table, the following fields should have some information: i. item_name = Product Title ii. item_description = Product Decription
