# Ingesting New Datasets into BigQuery || [GSP411](https://www.cloudskillsboost.google/focuses/3692?parent=catalog) ||

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝

### Run the following Commands in CloudShell

```
curl -LO https://raw.githubusercontent.com/Titash-shil/Ingesting-New-Datasets-into-BigQuery-GSP411/refs/heads/main/gsp411.csv

curl -LO raw.githubusercontent.com/Titash-shil/Ingesting-New-Datasets-into-BigQuery-GSP411/refs/heads/main/products.csv

bq mk ecommerce

gsutil mb gs://$DEVSHELL_PROJECT_ID/

gsutil cp products.csv gs://$DEVSHELL_PROJECT_ID/

gsutil cp gsp411.csv gs://$DEVSHELL_PROJECT_ID/


bq --location=US load --source_format=CSV --autodetect --skip_leading_rows=1 ecommerce.products gs://$DEVSHELL_PROJECT_ID/products.csv

bq --location=US load --source_format=CSV --autodetect --skip_leading_rows=1 ecommerce.products gs://data-insights-course/exports/products.csv


bq query --use_legacy_sql=false \
"
#standardSQL
SELECT
  *,
  SAFE_DIVIDE(orderedQuantity,stockLevel) AS ratio
FROM
  ecommerce.products
WHERE
# include products that have been ordered and
# are 80% through their inventory
orderedQuantity > 0
AND SAFE_DIVIDE(orderedQuantity,stockLevel) >= .8
ORDER BY
  restockingLeadTime DESC
"

cat > external_table_definition.json <<EOF
{
  "sourceFormat": "GOOGLE_SHEETS",
  "sourceUris": ["https://docs.google.com/spreadsheets/d/1Pyr2ifVgC82eCDNxBKgEXc33fkzMTPa2/edit?usp=sharing"],
  "schema": {
    "fields": [
      {"name": "column1", "type": "STRING"},
      {"name": "column2", "type": "INTEGER"},
      {"name": "column3", "type": "FLOAT"}
    ]
  }
}
EOF

bq mk --external_table_definition=external_table_definition.json ecommerce.products_comments
```

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
