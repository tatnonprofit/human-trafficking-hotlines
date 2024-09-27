# Human Trafficking Hotlines

This is a list of human trafficking hotlines in the USA, Canada, and Mexico, intended to be used by applications that want to use geolocation to show their users the most relevant hotlines that they can call to report instances of human trafficking. The list is vetted by TAT (Truckers Against Trafficking).

If you use this repository, please contact TAT at appdev@tatnonprofit.org. We request that if you use this data you also provide us with analytics. We would also like to keep in touch with you so we can share updates to this data as it becomes available. Information on how this data is used is vitally important to us as we seek to understand and measure our impact.

## Summary of the data

`hotlines.json` contains an array of hotlines (including the hotline's phone number, the name of the hotline, etc). Each hotline only serves a certain region, and that region is referred to by a commonly recognized region code (i.e. `US-NY`).

`regions.json` contains the GeoJSON data for each region referred to by the hotlines. The regions are indexed by region code. Import this file into your project if you don't already use a library in your project that can give you the region code of the user's geolocation.

`regions.custom-only.json` contains the GeoJSON data only for the regions that don't have commonly recognized region codes. If you don't import `regions.json`, you'll need to import this file into your project instead, to be able to determine whether the user is located in the custom regions defined by this project.

The `min` directory contains the same files as those listed above, but with whitespace removed for a smaller file size.

See the [Data format](#data-format) section below for more details on the structure of the data.

## Recommended usage

1. Determine the user's GPS coordinates
1. Filter the list of hotlines to those associated with regions that the user is currently in
1. Display the hotlines in the user's preferred language, sorted by ascending region size, with the "victim" and "emergency" hotlines at the end

## Data format

### Format of data in `hotlines.json`

#### Example

```json
[
    {
        "phoneNumber": "855-363-6548",
        "hotlineType": "state",
        "name": {
            "en-us": "New Jersey Human Trafficking Hotline",
            "es-mx": "Línea Directa de Trata de Personas de New Jersey",
            "fr-ca": "Ligne d'assistance téléphonique sur la traite des êtres humains au New Jersey"
        },
        "regionCode": "US-NJ",
        "supportedLanguages": [
            "en-us"
        ]
    },
    {
        ...
    }
]
```

#### Explanation

| Property | Description |
|----------|-------------|
| `phoneNumber` | A string representing the hotline's phone number |
| `hotlineType` | A brief description of what kind of hotline this is: one of `emergency`, `national`, `state`, `regional`, or `victim`.<br><br>**`emergency`** - a general-purpose emergency phone number <br><br>**`national`** - a human trafficking hotline that serves a whole country<br><br>**`state`** - a human trafficking hotline that only serves one state, province, territory, or district <br><br>**`regional`** - a human trafficking hotline that serves an area smaller than state/province/etc., such as a county or city <br><br>**`victim`** - a victim assistance hotline, not suitable for reporting trafficking tips |
| `name` | Contains the official name of the hotline in multiple languages, stored as object whose keys are ISO-639-1 language codes with country codes (in the format `xx-yy`), and whose values are the name of the hotline in the specified language. |
| `regionCode` | If the region is a country, this is the ISO 3166-1 alpha-2 code of the region (i.e. `US`). If the region is a state/province/territory, this is the ISO 3166-2 code of the region (i.e. `US-NY`). If the region has no standard ISO code, this value is a custom string which can be used to reference the region data in the `regions.json` file. |
| `supportedLanguages` | An array of ISO-639-1 language codes representing the languages spoken by the hotline's telephone agents |


### Format of data in `regions.json`

#### Example

```json
{
    "US-AZ": {
        "name": "Arizona",
        "code": "US-AZ",
        "geojson": { ... }
    },
    ...
}
```

#### Explanation

| Property | Description |
|----------|-------------|
| `name` | The name of the region in English |
| `code` | The 3166-1 alpha-2 or ISO 3166-2 code of the region, depending on whether it's a country or state/province/territory. If the region has no official ISO code, this is a custom string that is also used in `hotlines.json`. |
| `geojson` | The GeoJSON data describing the size and shape of the region. | 

