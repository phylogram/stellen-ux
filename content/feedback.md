---
title: Feedback
---
# Feedback

There is room for some improvements:

## Query-Params / Endpoints

- The naming of paths and query params is in mixes languages. Stick to one – english, as it is the currently lingua franca.
- Even as, a native german speaker the meaning of the paths and query params is not apparent: What is stelle? Why is text an array?
- There is no id field in `GET /api/stelle/?whatever=whatever`
- Types of query params are not given – for example how do search with `GET /api/stelle/?text=param`
- Kind of filter is not given: Is this a `like` or `=` search (or else)?
- I miss `order-by` features. 
- I guess, some of these issues would be address by using [swagger](https://swagger.io/), like [fast-api](https://fastapi.tiangolo.com/).

## Pagination

- Pagination is done before filtering? When adding `GET /api/stelle/?offset=0&limit=2&id=50` an empty result set is returned.
- The `count` field in `GET /api/stelle/?whatever=whatever` returns the count of the filtered result. An unfiltered result count would be usable on the client side.


