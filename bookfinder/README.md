# Project: Book Finder Full-Stack Application

## Background

When building a full-stack application, we're typically concerned with both a front end, that displays information to the user and takes in input, and a backend, that manages persisted information.

For this project, you will develop a book finder application that allows users to search for books by title, author, or ISBN. This project will use the Open Library API to fetch book data and display it interactively.

## Requirements

### 1: The user should be able to search for books.

Use the provided `searchBooks` function to allow the user to search for a book based on a title, isbn, or author. When this function is successfully implemented:

* The function should use its parameters to identify what value to search for and what type of search to perform.
* The function returns the books from the result of querying the Open Library API.
* If more than 10 books are returned from the API, the function’s returned data should be at most 10 books
* Each book returned should include the following properties:
  * title
  * author_name
  * isbn
  * cover_i
  * ebook_access
  * first_publish_year
  * ratings_sortable

### 2: Our application should be able to display book search results.

Given an array of book objects, the `displayBookList` function should be used to dynamically create `<li>` elements representing books. When this function is successfully implemented:

* Each `<li>` element should be within the unordered list that has an id of `book-list`. 
* Each `<li>` element should present the following data visually on the webpage:
  * the book’s title within an element that has a class of `title-element`
  * the book’s author within an element that has a class of `author-element`
  * The book’s cover within an element that has a class of `cover-element`
  * The book’s rating within an element that has a class of `rating-element`
  * The book’s e-book access value within an element that has a class of `ebook-element`
  * Note: the order or how the information is displayed is up to the developer.

### 3: The application should handle the event of the user performing a book search.

The HTML should include a `<form>` element with an id of `search-form` that includes, at minimum, the following:

* A textbox for the user to enter the value they are searching for that has an id of `search-input`
* A select element with an id of `search-type` for the user to choose the type of search
  * The types should be title, isbn, or author
* A submit button with an id of `submit-button`

In the `script.js` file, there should be a corresponding `handleSearch` function that is triggered when a user submits the above form. This function should ensure the Open Library is queried, meaning it should call the `searchBooks` function, and the application’s UI is updated accordingly, meaning it should call the `displayBookList` function.

### 4: The application should handle the event of the user clicking on a book returned from a  search result.

Once a user clicks on a book, there should be detailed book information displayed to the user about that specific book. This information should be contained in an HTML element of the developer’s choice, but it’s id must be `selected-book`. The following book data should be visually present within this element:

  * the book’s title within an element that has a class of `title-element`
  * the book’s author within an element that has a class of `author-element`
  * The book’s first publish year
  * The book’s cover
  * The book’s ebook access value
  * The book’s rating
  * The book’s ISBN
  * Note: the order or how the information is displayed is up to the developer.

In the `script.js` file, there should be the `displaySingleBook` function that is triggered when a book is clicked on by the user. This function is successfully implemented when:

* The HTML unordered list element with an id of `book-list` is hidden
* The HTML element with an id of `selected-book` should be visible

### 5:  Our application’s search results should be sortable by rating.

In your HTML page, you should include a button element with an id of `sort-rating`. This element, when clicked after a book search is made, should trigger the `handleSort` function so that books are displayed by rating in descending order (highest to lowest).

*  If any rating is non-numeric, such as undefined or unknown, the book's rating must be changed to "0" instead.

### 6: Our application’s search results should be filterable by whether or not the results are available as ebooks.

In your HTML page, you should include a checkbox element with an id of `ebook-filter`. This element, when checked after a book search is made, should trigger the `handleFilter` function so that only the books in the list of search results that are borrowable as e-books should be displayed.

When it is unchecked, any book, whether it is borrowable as an e-book or not, should be displayed.

### 7: Semantic elements should be included in HTML for web accessibility.

Any three of the following semantic elements should be included within the HTML webpage:

  * `<article>`
  * `<aside>`
  * `<details>`
  * `<figcaption>`
  * `<figure>`
  * `<footer>`
  * `<header>`
  * `<main>`
  * `<nav>`
  * `<section>`

### 8: CSS styling should be used to create a responsive web application.

In the `styles.css` file, any one of the following should be used:

* CSS grid
* media queries
* CSS Flexbox

---

# Using the Open Library API

### Basic Query Example

A basic query is as follows:

`https://openlibrary.org/search.json?q=searchterm`

Where `q` is a query parameter that represents the key “query” and the associated value is the search term you want to use. 

Below is the data format returned from a query:
```bash
{
  "numFound": 0,
  "start": 0,
  "numFoundExact": true,
  "docs": [],
  "num_found": 0,
  "q": "",
  "offset": null
}

``` 

`docs ` will contain any books returned.


For example, if I want to search for “harry potter”, I can use the following URL:

`https://openlibrary.org/search.json?q=harry%20potter`

Note that the characters %20 represent a space.

The data returned is as follows:
```bash
{
  "numFound": 3532,
  "start": 0,
  "numFoundExact": true,
  "docs": [...], // omitted, because 3000+ books were returned!
  "num_found": 3532,
  "q": "harry potter",
  "offset": null
}
```

### Getting Specific Book Fields

Be default, a book is returned with many fields, most of which you won’t be using. Below is an example of book information taken from the harry potter search we used earlier:

```bash
  // this is from the first book returned from the previous query:
  "author_alternative_name": [],
  "author_key": [],
  "author_name": [],
  "contributor": [],
  "cover_edition_key": "OL22856696M",
  "cover_i": 10521270,
  "ddc": [],
  "ebook_access": "borrowable",
  "ebook_count_i": 59,
  "edition_count": 328,
  "edition_key": [],
  "first_publish_year": 1997,
  "first_sentence": [],
  "format": [],
  "has_fulltext": true,
  "ia": [],
  "ia_collection": [],
  ... // the rest are omitted
```

In order to specify the fields we want returned, we can use the “fields” query parameter. You can pass in comma-separated values represented the fields you actually want returned. Below is an example:

```bash
https://openlibrary.org/search.json?q=harry%20potter&fields=title,%20author_name,%20ebook_access,%20first_publish_year,ratings_sortable,isbn&sort=rating&limit=5
```

The above query harry potter books with the following fields returned:

* title
* author name
* ebook access
* first year published
* rating
* list of related isbn’s

We also include query parameters to limit the amount of books returned to be 5, and they are sorted by their rating.

The data returned is as follows:
```bash
{
  "numFound": 3531,
  "start": 0,
  "numFoundExact": true,
  "docs": [
  {
    "author_name": [
    "J. K. Rowling"
    ],
    "ebook_access": "borrowable",
    "first_publish_year": 2005,
    "isbn": [...], // values omitted, there's a lot!
    "title": "Harry Potter and the Half-Blood Prince",
    "ratings_sortable": 4.234534
  },
  {
    "author_name": [
    "J. K. Rowling"
    ],
    "ebook_access": "no_ebook",
    "first_publish_year": 1999,
    "isbn": [...], // values omitted, there's a lot!
    "title": "Harry Potter (series) 1-7",
    "ratings_sortable": 4.2184067
  },
  {
    "author_name": [
    "J. K. Rowling"
    ],
    "ebook_access": "borrowable",
    "first_publish_year": 1999,
    "isbn": [...], // values omitted, there's a lot!
    "title": "Harry Potter and the Prisoner of Azkaban",
    "ratings_sortable": 4.2145524
  },
  {
    "author_name": [
    "J. K. Rowling"
    ],
    "ebook_access": "borrowable",
    "first_publish_year": 1993,
    "isbn": [...], // values omitted, there's a lot!
    "title": "Harry Potter and the Goblet of Fire",
    "ratings_sortable": 4.1933813
  },
  {
    "author_name": [
    "J. K. Rowling"
    ],
    "ebook_access": "borrowable",
    "first_publish_year": 2003,
    "isbn": [...], // values omitted, there's a lot!
    "title": "Harry Potter and the Order of the Phoenix",
    "ratings_sortable": 4.183708
  }],
  "num_found": 3531,
  "q": "harry potter",
  "offset": null
}
```

### Searching Using a Specific Field

You can specify what type of field you want to search for, rather than a general query (i.e. rather than using the `q` query parameter). Below is an example of specifying that you’re searching by the field `isbn`: 

```bash
https://openlibrary.org/search.json?isbn=9788478886135
```

### Getting a Cover 

The URL pattern to access book covers is: `https://covers.openlibrary.org/b/$key/$value-$size.jpg`

Where:
* key can be any one of ISBN, OCLC, LCCN, OLID and ID (case-insensitive)
* value is the value of the chosen key
* size can be one of S, M and L for small, medium and large respectively.

By default it returns a blank image if the cover cannot be found. If you append `?default=false` to the end of the URL, then it returns a `404` instead.

The following example returns small sized cover image for book with ISBN `0385472579`.

`https://covers.openlibrary.org/b/isbn/0385472579-S.jpg`

Below is a harry potter cover image, size large:

`https://covers.openlibrary.org/b/isbn/9788478886135-L.jpg`

Good luck!