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
			"searchUrl": "/search?page={pageNumber}&genre={genre}&status={status}",
			"container": ".search-results",
			"title": ".manga-title",
			"url": ".manga-link",
			"poster": "img.poster",
			"placeholderValues": {
				"genre": "Action,Adventure,...",
				"status": "publishing,finished,..."
			}
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
		"volumeRegex": "Volume (\\d+(\\.\\d+)?)", // Optional
		"statusMapping": {
			"ongoing": "ONGOING",
			"completed": "COMPLETED",
			"hiatus": "HIATUS",
			"dropped": "DROPPED",
			"_default": "ONGOING"
		}
	},
	"imageConfig": {
		"referrer": "https://example.com" // Optional: for sites that need specific referrer
	},
	"enabled": true
}
```

### Fields Explanation

#### Base Configuration

-   `name`: The display name of the provider
-   `baseUrl`: The base URL of the manga site. This is crucial for constructing full URLs
-   `enabled`: The default enabled status

#### Selectors

-   `search`: Selectors for the search page

    -   `searchUrl`: URL template for search (supports placeholders like {pageNumber}, {genre}, {status})
    -   `container`: Container element for each manga in search results
    -   `title`: Title element selector
    -   `url`: URL element selector (if different from container)
    -   `poster`: Poster image selector
    -   `placeholderValues`: Optional mapping of placeholder values for advanced search, not needed for pageNumber,
        which is 1 to X

-   `details`: Selectors for manga details page

    -   `title`: Manga title selector
    -   `poster`: Cover image selector
    -   `description`: Description text selector
    -   `authors`: Authors text selector (can be empty string or "no" if not available)
    -   `genres`: Genres elements selector
    -   `status`: Status text selector
    -   `seriesType`: Series type selector (manga/manhwa/etc)

-   `chapters`: Selectors for chapter list

    -   `container`: Container element for each chapter
    -   `title`: Chapter title selector
    -   `number`: Chapter number selector (optional)
    -   `date`: Release date selector
    -   `url`: Chapter URL selector (can be empty if container is clickable)

-   `pages`: Selectors for chapter pages
    -   `container`: Image container selector
    -   `imageAttribute`: Attribute containing image URL (usually "src" or "data-src")

#### Parsing Configuration

-   `dateFormat`: How to parse dates

    -   `standard`: Regular date format
    -   `noOp`: Return current date
    -   `ordinal`: Handle dates with ordinals (1st, 2nd, etc)

-   `chapterRegex`: Regular expression for extracting chapter numbers

    -   Must include at least one capture group for the chapter number
    -   Optional decimal support with (\\.\\d+)?

-   `volumeRegex`: Optional regular expression for extracting volume numbers

    -   Similar structure to chapterRegex
    -   Used when series has volume information

-   `statusMapping`: Map provider status text to MangaScroll status
    -   Keys: Provider's status text (lowercase)
    -   Values: One of: "ONGOING", "COMPLETED", "HIATUS", "DROPPED", "SEASON_END"
    -   `_default`: Default status if no match found

#### Image Configuration

-   `imageConfig`: Optional configuration for image loading
    -   `referrer`: Referrer URL for image requests
    -   Can be null if no special configuration is needed

## Contributing

1. Fork this repository
2. Create a new JSON file in the `providers` directory
3. Follow the configuration structure above
4. Test your configuration
5. Submit a Pull Request

The GitHub Action will automatically generate the combined `providers.json` file when changes are merged.

## License

MIT License
