# Errors

The cloco API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is badly formed
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- You do not have provilege to access the requested URL
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a kitten with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
409 | Conflict -- Your request breaches subscription rules
418 | I'm a teapot
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
