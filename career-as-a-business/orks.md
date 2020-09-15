Conversion rate = (conversions / total visitors) * 100%

```
#Gumroad export
$purchases = (Import-Csv ./purchase.csv).'Buyer Email'

#Convertkit export
$signups = (Import-Csv ./signups.csv).email

$converted = $purchases | ForEach-Object { if ($signups -contains $_){ $_}}

$conversionRate = ($converted.count / 395) * 100
```
