import requests

# Step 1: Ask user to enter the filename
filename = input("Enter the path of your rsID text file: ")

# Step 2: Read all rsIDs from the file
with open(filename, "r") as file:
    rsids = [line.strip() for line in file if line.strip()]

# Step 3: Loop through each rsID
for rsid in rsids:
    url = f"https://rest.ensembl.org/variation/human/{rsid}?"
    headers = {"Content-Type": "application/json"}
    response = requests.get(url, headers=headers)

    # Step 4: Check for valid response and JSON
    if response.status_code == 200 and response.text.strip():
        try:
            data = response.json()

            print(f"\n Info for {rsid}:")
            print(f"  Most Severe Consequence: {data.get('most_severe_consequence', 'N/A')}")

            genes = [m.get('gene_symbol') for m in data.get('mappings', []) if m.get('gene_symbol')]
            print(f"  Mapped Genes: {genes if genes else 'N/A'}")

            print(f"  Ancestral Allele: {data.get('ancestral_allele', 'N/A')}")
            print(f"  Clinical Significance: {', '.join(data.get('clinical_significance', ['N/A']))}")
            print(f"  Minor Allele: {data.get('minor_allele', 'N/A')}")
            print(f"  Minor Allele Frequency: {data.get('minor_allele_freq', 'N/A')}")

            synonyms = ", ".join(data.get('synonyms', [])) if data.get('synonyms') else "N/A"
            print(f"   Synonyms: {synonyms}")

        except requests.exceptions.JSONDecodeError:
            print(f"\n Response for {rsid} is not valid JSON. Skipping.")
            continue

    else:
        print(f"\nFailed to retrieve info for {rsid} (HTTP {response.status_code})")

