# SEC Entity Shares Outstanding Viewer

This is a single-file, responsive HTML application built with Tailwind CSS and vanilla JavaScript. It fetches and displays the maximum and minimum common stock shares outstanding for a given SEC CIK (Central Index Key) from the SEC's EDGAR API.

## Features

-   **Dynamic Data Fetching**: Retrieves `EntityCommonStockSharesOutstanding` data directly from the SEC EDGAR API.
-   **CIK Parameter Support**: Can load data for any specified CIK via a URL query parameter (`index.html?CIK=0001018724`). Defaults to Amphenol's CIK (`0000820313`).
-   **Data Filtering**: Filters shares outstanding data to include only fiscal years (`fy`) greater than "2020" and ensures `val` is a numeric type.
-   **Max/Min Identification**: Clearly identifies and displays the highest and lowest share values along with their corresponding fiscal years.
-   **Responsive Design**: Uses Tailwind CSS for a clean, modern, and mobile-friendly user interface.
-   **Loading and Error States**: Provides visual feedback during data fetching and displays informative error messages if something goes wrong.

## How to Use

1.  **Save the file**: Save the provided `index.html` content as `index.html` in a local directory.
2.  **Open in Browser**: Open the `index.html` file directly in your web browser.

    *   **Default CIK (Amphenol)**: If you open `index.html` without any parameters, it will display data for Amphenol (CIK: `0000820313`).
    *   **Custom CIK**: To view data for a different company, append a `CIK` query parameter to the URL. For example:
        `file:///path/to/your/index.html?CIK=0001018724` (for Apple Inc.)

## Technical Details

### Data Source

The application fetches data from the SEC EDGAR API endpoint:
`https://data.sec.gov/api/xbrl/companyconcept/CIK<CIK_PLACEHOLDER>/dei/EntityCommonStockSharesOutstanding.json`

### Proxy Usage

Due to browser same-origin policy and SEC's requirement for a descriptive `User-Agent` header (which cannot be set directly by client-side JavaScript), this application uses `https://api.allorigins.win/get?url=` as a public CORS proxy.

**Important Note on User-Agent Compliance:**
While `allorigins.win` helps bypass CORS, it does not allow setting a custom, descriptive `User-Agent` as mandated by SEC guidance for programmatic access. For a production-grade application, it is highly recommended to implement a custom backend proxy server that can correctly set the `User-Agent` header before forwarding requests to the SEC API. Public proxies may also have rate limits or stability issues.

### Data Processing Logic

The JavaScript code performs the following steps:
1.  Retrieves the CIK from the URL query parameter or uses a default value.
2.  Constructs the SEC API URL and passes it through the `allorigins.win` proxy.
3.  Parses the JSON response.
4.  Filters the `units.shares` array:
    *   Only entries with `fy` (fiscal year) greater than "2020" are considered.
    *   Only entries where `val` (value) is a numeric type are included.
5.  Iterates through the filtered shares to find the `max` and `min` `val` along with their associated `fy`.
6.  Updates the DOM elements with the fetched entity name and the determined max/min shares outstanding.

## Project Structure

```
.
├── index.html        # The single-file responsive HTML application
├── README.md         # This README file
└── LICENSE           # MIT License text
```

*Note: The `uid.txt` file mentioned in the prompt is a provided attachment and not directly embedded or displayed within this single-file HTML application's code or output JSON.*

## Credits

-   **Tailwind CSS**: For utility-first CSS styling.
-   **SEC EDGAR API**: For providing financial data.
-   **allorigins.win**: For providing a public CORS proxy (for development/demonstration purposes).