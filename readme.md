# Focus Data Presentation Azure Composition
This composition is part of the FinOps Data Presentation component, in the Krateo Composable FinOps.

## Summary

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Examples](#examples)
4. [Configuration](#configuration)

## Overview
This composition allows for the creation of a single resource, the `DataPresentationAzure` and let the [Composition Dynamic Controller](https://github.com/krateoplatformops/composition-dynamic-controller) automatically compile the FocusConfig resource from the [finops-operator-focus](https://github.com/krateoplatformops/finops-operator-focus), which also starts the exporting/scraping process, automatically storing data into the database. See [finops-operator-exporter](https://github.com/krateoplatformops/finops-operator-exporter) and [finops-operator-scraper](https://github.com/krateoplatformops/finops-operator-scraper) for more information.

## Architecture
In the diagram, this component is the `Focus Data Presentation Composition` (in this specific instance, the provider is Azure).

![FinOps Composition Definition Parser](_diagrams/architecture.png)

## Examples
The configuration requires the filter for the `DataPresentationAzure` resource, and the `ScraperConfig`, to allow the data to be stored into the database (**note**: the `tableName` must be set to _pricing_table_, or consider modifying the notebook on the finops-database-handler for frontend presentation):
```yaml
apiVersion: composition.krateo.io/v0-1-0
kind: FocusTemplate
metadata:
  name: sample
  namespace: krateo-system
spec:
  filter: serviceFamily eq 'Compute' and armRegionName eq 'westeurope' and skuId eq 'DZH318Z08NRP/001B' and type eq 'Consumption'
  scraperConfig:
    tableName: pricing_table # This table name is currently mandatory
    pollingIntervalHours: 1
    scraperDatabaseConfigRef: 
      name: cratedb-config
      namespace: krateo-system
```

## Configuration
Check the Krateo FinOps Operators for additional information on how to compile the `ScraperConfig`:
- [finops-operator-focus](https://github.com/krateoplatformops/finops-operator-focus)
- [finops-operator-exporter](https://github.com/krateoplatformops/finops-operator-exporter)
- [finops-operator-scraper](https://github.com/krateoplatformops/finops-operator-scraper)