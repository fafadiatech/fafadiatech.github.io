# Web API Desing - Crafting Interfaces that Developers Love

API consumption is increasing day by day. Design becomes an important element from maintainability and usability prespective. Following is compilation of [Web API Design by apigee](https://apigee.com/about/resources/ebooks/web-api-design)

## Nouns are god; verbs are bad

- Keep your base URL simple and intitive. Affordance is design property that communicate how something should be used without requiring documentation
	- Simple rules is to have 2 base URLs per resources
		- First for collection eg. /dogs
		- Second for a specific element in the collections e.g. /dogs/123
- Keep verbs out of your base URLs
- Use HTTP verbs to operate on collections and elements
	- POST -> Create
	- GET -> Read
	- PUT - Update
	- DELETE - Delete

## Plural nouns and concrete names

- In production both singular and plural forms are used
	- Foursquare: /checkins
	- GroupOn: /deals
	- Zappos: /Product
- Be consistent allow developers to predict and guess the method calls
- Conceret names are better than abstract
	- Achieving pure abstraction is sometimes a goal of API architects, its not always meaningful to developers. 
	- E.g. blogs, videos and news are more meaningful than items, assets

## Simplify associations - sweep complexity under the "?"

- Association: Lets say you want to associate dogs with their owners we can use GET and POST verbs in pattern of `resource/identifier/resource`
	- GET /owners/5678/dogs
	- POST /owners/5678/dogs
- Sweep complexity behind the "?"
	- Complexities can be include many state that can be updated, changed, queries as well as the attributes associated with a resource.
	- E.g. /dogs?color=red&state=running&location=park

## Handling errors

- Since APIs are blackboxes, error become key tool in providing context and visibilty
- Use HTTP status codes:
	- 200: Everything worked
	- 400: Bad Request
	- 401: Unauthorized
	- 403: Forbidden
	- 404: Not Found
	- 500: Internal Server Error

## MISC tips

- Versioning: Never release an API without a version and make the version mandatory
- Pagination: Use of limit and offset values with optional field values
- Make it clear in your API documentation that "non-resource" scenarios are different.
	- E.g. Calculator
- Support Multiple Formats: E.g. JSON/XML
