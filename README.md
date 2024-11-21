# MangaScroll Providers

This repository contains provider configurations for MangaScroll. Each provider defines how to scrape manga information
from different websites.

## Structure

-   `/providers/` - Individual provider configuration files
-   `providers.json` - Auto-generated combined file containing all providers

## Provider Configuration

Each provider is defined in a JSON file with the following structure:

```json
{
	"name": "ProviderName",
	"baseUrl": "https://example.com",
	"selectors": {
		"search": {
			"searchUrl": "/search?page={pageNumber}",
			"container": ".search-results",
			"title": ".manga-title",
			"url": ".manga-link",
			"poster": "img.poster"
		},
		"details": {
			"title": "h1.title",
			"poster": "img.cover",
			"description": ".description",
			"authors": ".author",
			"genres": ".genres",
			"status": ".status",
			"seriesType": ".type"
		},
		"chapters": {
			"container": ".chapter-list",
			"title": ".chapter-title",
			"number": ".chapter-number",
			"date": ".chapter-date",
			"url": ".chapter-link"
		},
		"pages": {
			"container": ".page-image",
			"imageAttribute": "src"
		}
	},
	"parsing": {
		"dateFormat": "standard | noOp | ordinal",
		"chapterRegex": "Chapter (\\d+(\\.\\d+)?)",
		"statusMapping": {
			"ongoing": "ONGOING",
			"completed": "COMPLETED",
			"_default": "ONGOING"
		}
	},
	"imageConfig": {
		"referrer": "https://example.com" // Optional: for sites that need specific referrer
	}
}
```

### Fields Explanation

#### Selectors

-   `search`: Selectors for the search page

    -   `searchUrl`: URL template for search (supports {pageNumber} placeholder)
    -   `container`: Container element for each manga in search results
    -   `title`: Title element selector
    -   `url`: URL element selector (if different from container)
    -   `poster`: Poster image selector

-   `details`: Selectors for manga details page

    -   `title`: Manga title selector
    -   `poster`: Cover image selector
    -   `description`: Description text selector
    -   `authors`: Authors text selector
    -   `genres`: Genres elements selector
    -   `status`: Status text selector
    -   `seriesType`: Series type selector (manga/manhwa/etc)

-   `chapters`: Selectors for chapter list

    -   `container`: Container element for each chapter
    -   `title`: Chapter title selector
    -   `number`: Chapter number selector
    -   `date`: Release date selector
    -   `url`: Chapter URL selector

-   `pages`: Selectors for chapter pages
    -   `container`: Image container selector
    -   `imageAttribute`: Attribute containing image URL

#### Parsing Configuration

-   `dateFormat`: How to parse dates

    -   `standard`: Regular date format
    -   `noOp`: Return current date
    -   `ordinal`: Handle dates with ordinals (1st, 2nd, etc)

-   `chapterRegex`: Regular expression for extracting chapter numbers

    -   Must include at least one capture group for the chapter number
    -   Optional first capture group for volume number

-   `statusMapping`: Map provider status text to MangaScroll status
    -   Keys: Provider's status text (lowercase)
    -   Values: One of: "ONGOING", "COMPLETED", "HIATUS", "DROPPED", "SEASON_END"
    -   `_default`: Default status if no match found

#### Image Configuration

-   `imageConfig`: Optional configuration for image loading
    -   `referrer`: Referrer URL for image requests

## Contributing

1. Fork this repository
2. Create a new JSON file in the `providers` directory
3. Follow the configuration structure above
4. Test your configuration
5. Submit a Pull Request

The GitHub Action will automatically generate the combined `providers.json` file when changes are merged.

## License

MIT License
