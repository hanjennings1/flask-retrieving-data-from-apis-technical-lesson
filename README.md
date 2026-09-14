# Flask: Retrieving Data from an API — Technical Lesson
**Status:** ✅ Completed - September 14, 2026

A small Python script that demonstrates how to retrieve and parse data from
a RESTful API. It queries the [Open Library Search API](https://openlibrary.org/dev/docs/api/search)
for a book title entered by the user and prints back the book's title and
author.

## Description

This project was built to explore how to interact with a public REST API
using Python's `requests` library. It covers constructing a query URL with
parameters, sending a GET request, parsing the JSON response, and formatting
the result for a user.

Given a book title, the script:

1. Formats the title into a URL-safe query string.
2. Sends a GET request to the Open Library `search.json` endpoint.
3. Parses the JSON response.
4. Returns the title and author of the top matching result.

## Getting Started

### Dependencies

* Python 3.8+
* [pipenv](https://pipenv.pypa.io/en/latest/)
* [requests](https://requests.readthedocs.io/en/latest/)

### Installing

1. Fork and clone this repository.
2. Install dependencies:

   ```bash
   pipenv install
   ```

3. Activate the virtual environment:

   ```bash
   pipenv shell
   ```

### Executing the Program

Run the script from the project root:

```bash
python lib/open_library_api.py
```

You'll be prompted to enter a book title:

```
Enter a book title: the lord of the rings
```

The script will return the top result:

```
Search Result:

Title: The Lord of the Rings
Author: J.R.R. Tolkien
```

## How It Works

The core logic lives in `lib/open_library_api.py`, in the `Search` class:

```python
class Search:

    def get_search_results(self, search_term):
        search_term_formatted = search_term.replace(" ", "+")
        fields = ["title", "author_name"]
        fields_formatted = ",".join(fields)
        limit = 1

        URL = f"https://openlibrary.org/search.json?title={search_term_formatted}&fields={fields_formatted}&limit={limit}"

        response = requests.get(URL).json()
        response_formatted = f"Title: {response['docs'][0]['title']}\nAuthor: {response['docs'][0]['author_name'][0]}"
        return response_formatted
```

* **`search_term`** is passed in from user input rather than hardcoded, so
  the script can look up any title.
* Spaces in the search term are replaced with `+` to keep the URL valid.
* The `fields` list limits the response to just the data we need (`title`
  and `author_name`), keeping the payload small.
* `limit=1` restricts the response to a single, best-matching book.
* `.json()` converts the raw response into a Python dictionary so the data
  can be accessed and formatted easily.

## Example API Request

```
https://openlibrary.org/search.json?title=the+lord+of+the+rings&fields=title,author_name&limit=1
```

## Acknowledgments

* [Open Library API Docs](https://openlibrary.org/dev/docs/api/search)
* [Python Requests Library](https://requests.readthedocs.io/en/latest/)
* Lesson structure adapted from the Flask: Retrieving Data from an API
  technical lesson.