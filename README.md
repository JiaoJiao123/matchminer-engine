# MatchEngine
Clinical trial matching algorithm.

## Requirements
MatchEngine requires you to have Python 2.7 and MongoDB installed and running. We recommend using virtualenv to create a fresh Python2.7 enviroment, all additional required python libraries can be installed with
```bash
pip install -r requirements.txt
```

## User Guide
### Setting up MongoDB
MongoDB must be installed for MatchEngine to execute. For MongoDB installation instructions
for Linux, Mac OS X, and Windows please visit:
https://docs.mongodb.com/manual/administration/install-community/

### Set Up
MatchEngine matches clinical and genomic data to clinical trials in a Mongo
database. To set up the database for matching run the command:
```bash
python matchengine.py load -t examples/trial.bson -c examples/clinical.example.csv -g examples/genomic.example.csv --mongo-uri $your_mongo_uri
```

Example trial, clinical and genomic tables are located in the "examples" directory.

You have the option to change the default input file formats.
Trials can be inserted from yaml files by setting the --yml flag, and
clinical and genomic data can be inserted from pickle files by setting the --pkl flag.
Otherwise, the default expected file formats are a .bson file for trials
and csv files for clinical and genomic data.

Clinical csvs required the following columns:
- DFCI_MRN (Unique patient identifier)
- SAMPLE_ID (Unique sample identifier)
- ONCOTREE_PRIMARY_DIAGNOSIS_NAME (Disease diagnosis)
- BIRTH_DATE (Date of birth in format 'YYYY-MM-DD 00:00:00.000')

Suggested additional columns that will be included in the match results are:
- ORD_PHYSICIAN_NAME
- ORD_PHYSICIAN_EMAIL
- REPORT_DATE
- VITAL_STATUS (alive or deceased)
- FIRST_LAST (Patient's first and last name)

Genomic csvs required the following columns:
- SAMPLE_ID (Unique sample identifier)
- TRUE_HUGO_SYMBOL (Gene name)
    
Additional columns used in matching are:
- TRUE_PROTEIN_CHANGE (Specific variant)
- TRUE_VARIANT_CLASSIFICATION (Variant type)
- VARIANT_CATEGORY (CNV, MUTATION, or SV)
- TRUE_TRANSCRIPT_EXON (Exon number [integer])
- CNV_CALL (Heterozygous deletion, Homozygous deletion, Gain, High Level amplification, or null)
- WILDTYPE (True or False)

Suggested additional fields that will be included in the match results are:
- CHROMOSOME (Chromosome number in format 'chr01')
- POSITION [integer]
- TRUE_CDNA_CHANGE
- REFERENCE_ALLELE
- CANONICAL_STRAND (- or +)
- ALLELE_FRACTION [float]
- TIER [integer]
    
Any additional columns beyond this list will be ignored by MatchEngine during matching.

### Matching
Once your MongoDB is set up you can perform matching by:
```bash
python matchengine.py match --mongo-uri $your_mongo_uri
```

Default output will be a csv file called "results.csv" in your current working directory.
You can specify the outpath path and filename of the results by setting the -o flag. NOTE: If using -o, please specify output directy AND filename. 
You can change the file format of the output to json by setting the --json flag.
