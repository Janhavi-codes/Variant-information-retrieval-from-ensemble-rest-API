Variant Info Retrieval using Ensembl REST API
This Python script reads a list of rsIDs (SNP IDs) from a text file and retrieves detailed variant annotation information using the Ensembl REST API.

Features
Takes a .txt file containing a list of rsIDs as input

Fetches variant annotations such as:

Most severe consequence

Mapped genes

Ancestral allele

Clinical significance

Minor allele & its frequency

Synonyms

Handles missing or malformed responses

Requirements
Python 3.x

requests library
