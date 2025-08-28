# End of Life integration

This integration is for [End of Life](https://endoflife.date/) logs. 

>endoflife.date aggregates data from various sources and presents it in an understandable and succinct manner 
>
>-endoflife.date

This data can be useful for current awareness of current End-of-life (EOL) products according to the open source project The integration periodically checks for the latest EOL product list. It includes the following datasets for retrieving logs from the End-of-life website:

- `product` dataset: Supports products classified as end-of-life.

## Logs

### End of life (EOL)

The End of life data_stream retrieves product information from the endpoint `https://endoflife.date/api/v1/products/full`.

{{event "product"}}

{{fields "product"}}