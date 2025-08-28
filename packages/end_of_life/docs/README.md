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

An example event for `product` looks as following:

```json
{
    "ecs": {
        "version": "9.1.0"
    },
    "end_of_life": {
        "aliases": [
            "ubuntu-linux"
        ],
        "category": "os",
        "identifiers": [
            {
                "id": "cpe:/o:canonical:ubuntu_linux",
                "type": "cpe"
            }
        ],
        "label": "Ubuntu",
        "labels": {
            "discontinued": "Discontinued",
            "eoas": "Hardware & Maintenance",
            "eoes": "Extended Security Maintenance",
            "eol": "Maintenance & Security Support"
        },
        "links": {
            "html": "https://endoflife.date/ubuntu",
            "icon": "https://simpleicons.org/icons/ubuntu.svg",
            "releasePolicy": "https://wiki.ubuntu.com/Releases"
        },
        "name": "ubuntu",
        "releases": [
            {
                "codename": "Jammy Jellyfish",
                "custom": {
                    "chromeVersion": "M136",
                    "nodeVersion": "22.15"
                },
                "discontinuedFrom": "2027-04-01",
                "eoasFrom": "2024-09-30",
                "eoesFrom": "2032-04-09",
                "eolFrom": "2027-04-01",
                "isDiscontinued": false,
                "isEoas": false,
                "isEoes": true,
                "isEol": false,
                "isLts": true,
                "isMaintained": true,
                "label": "22.04 'Jammy Jellyfish' (LTS)",
                "latest": {
                    "date": "2022-04-21",
                    "link": "https://wiki.ubuntu.com/JammyJellyfish/ReleaseNotes/",
                    "name": "22.04.2"
                },
                "ltsFrom": "2022-04-21",
                "name": "22.04",
                "releaseDate": "2022-04-21"
            }
        ],
        "tags": [
            "canonical",
            "os"
        ],
        "versionCommand": "lsb_release --release"
    },
    "event": {
        "category": [
            "vulnerability"
        ],
        "kind": "enrichment",
        "original": "{\"name\":\"ubuntu\",\"label\":\"Ubuntu\",\"aliases\":[\"ubuntu-linux\"],\"category\":\"os\",\"tags\":[\"canonical\",\"os\"],\"versionCommand\":\"lsb_release --release\",\"identifiers\":[{\"id\":\"cpe:/o:canonical:ubuntu_linux\",\"type\":\"cpe\"}],\"labels\":{\"eoas\":\"Hardware & Maintenance\",\"discontinued\":\"Discontinued\",\"eol\":\"Maintenance & Security Support\",\"eoes\":\"Extended Security Maintenance\"},\"links\":{\"icon\":\"https://simpleicons.org/icons/ubuntu.svg\",\"html\":\"https://endoflife.date/ubuntu\",\"releasePolicy\":\"https://wiki.ubuntu.com/Releases\"},\"releases\":[{\"name\":\"22.04\",\"codename\":\"Jammy Jellyfish\",\"label\":\"22.04 'Jammy Jellyfish' (LTS)\",\"releaseDate\":\"2022-04-21\",\"isLts\":true,\"ltsFrom\":\"2022-04-21\",\"isEoas\":false,\"eoasFrom\":\"2024-09-30\",\"isEol\":false,\"eolFrom\":\"2027-04-01\",\"isDiscontinued\":false,\"discontinuedFrom\":\"2027-04-01\",\"isEoes\":true,\"eoesFrom\":\"2032-04-09\",\"isMaintained\":true,\"latest\":{\"name\":\"22.04.2\",\"date\":\"2022-04-21\",\"link\":\"https://wiki.ubuntu.com/JammyJellyfish/ReleaseNotes/\"},\"custom\":{\"chromeVersion\":\"M136\",\"nodeVersion\":\"22.15\"}}]}",
        "type": [
            "info"
        ]
    },
    "tags": [
        "preserve_original_event"
    ]
}
```

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| end_of_life.aliases | List of alternative names or aliases. | keyword |
| end_of_life.category | The category of the product (e.g., os). | keyword |
| end_of_life.identifiers.id | Identifier value (e.g., CPE). | keyword |
| end_of_life.identifiers.type | Type of identifier (e.g., cpe). | keyword |
| end_of_life.label | The display label for the product or project. | keyword |
| end_of_life.labels.discontinued | Label for Discontinued. | keyword |
| end_of_life.labels.eoas | Label for Hardware & Maintenance. | keyword |
| end_of_life.labels.eoes | Label for Extended Security Maintenance. | keyword |
| end_of_life.labels.eol | Label for Maintenance & Security Support. | keyword |
| end_of_life.links.html | Main product page URL. | keyword |
| end_of_life.links.icon | URL to the product icon. | keyword |
| end_of_life.links.releasePolicy | URL to the release policy. | keyword |
| end_of_life.name | The short name of the product or project. | keyword |
| end_of_life.releases.codename | Codename of the release. | keyword |
| end_of_life.releases.custom.chromeVersion | Chrome version for the release. | keyword |
| end_of_life.releases.custom.nodeVersion | Node.js version for the release. | keyword |
| end_of_life.releases.discontinuedFrom | Discontinued start date. | date |
| end_of_life.releases.eoasFrom | Hardware & Maintenance start date. | date |
| end_of_life.releases.eoesFrom | Extended Security Maintenance start date. | date |
| end_of_life.releases.eolFrom | Maintenance & Security Support start date. | date |
| end_of_life.releases.isDiscontinued | Whether the release is discontinued. | boolean |
| end_of_life.releases.isEoas | Whether the release is in Hardware & Maintenance. | boolean |
| end_of_life.releases.isEoes | Whether the release is in Extended Security Maintenance. | boolean |
| end_of_life.releases.isEol | Whether the release is in Maintenance & Security Support. | boolean |
| end_of_life.releases.isLts | Whether the release is LTS. | boolean |
| end_of_life.releases.isMaintained | Whether the release is maintained. | boolean |
| end_of_life.releases.label | Display label for the release. | keyword |
| end_of_life.releases.latest.date | Latest release date. | date |
| end_of_life.releases.latest.link | URL to the latest release notes. | keyword |
| end_of_life.releases.latest.name | Latest release name. | keyword |
| end_of_life.releases.ltsFrom | LTS start date. | date |
| end_of_life.releases.name | Release version name. | keyword |
| end_of_life.releases.releaseDate | Release date. | date |
| end_of_life.tags | List of tags associated with the product. | keyword |
| end_of_life.versionCommand | Command to get the version of the product. | keyword |
| input.type | Type of Filebeat input. | keyword |
