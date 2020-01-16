# Errors

The qiibee API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- Your authorization header or API key is wrong.
404 | Not Found -- The specified resource cannot be found.
409 | Conflict -- The submitted resource (eg. a transaction) conflicts with an already existing resource.
429 | Too Many Requests
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
