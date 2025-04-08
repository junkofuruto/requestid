# Request ID middleware

The `requestid` package provides a middleware for HTTP servers in Go that manages a unique request identifier (Request ID) for each incoming HTTP request. This identifier can be used for tracking, logging, and debugging purposes.

## Features

* Extracts a Request ID from the HTTP headers.
* Generates a new UUID as a Request ID if none is provided.
* Stores the Request ID in the request context for easy access throughout the request lifecycle.

## Installation

To use the `requestid` package, ensure you have Go installed on your machine. You can include the package in your project by running:

``` bash
    go get github.com/junkofuruto/requestid
```

## Usage
### Middleware

To use the `requestid` middleware, wrap your HTTP handler with it. The middleware will check for a Request ID in the headers and generate one if it is not present.

``` go 
package main

import (
	"net/http"
	"github.com/junkofuruto/requestid"
)

func main() {
	http.Handle("/", requestid.RequestID(http.HandlerFunc(yourHandler)))
	http.ListenAndServe(":8080", nil)
}

func yourHandler(w http.ResponseWriter, r *http.Request) {
	requestID := requestid.GetRequestID(r.Context())
	w.Write([]byte("Your Request ID: " + requestID))
}
```

### Retrieving the Request ID

You can retrieve the generated or provided Request ID from the request context using the GetRequestID function:

``` go
func yourHandler(w http.ResponseWriter, r *http.Request) {
	requestID := requestid.GetRequestID(r.Context())
	// Use the requestID for logging or tracking
}
```

## How It Works
* The middleware checks for the Request ID in the X-Request-Id header.
* If a Request ID is not found, it generates a new UUID using the github.com/google/uuid package.
* The Request ID is stored in the request context, allowing it to be accessed later in the request handling process.
* If the context is nil or the Request ID is not found, an empty string is returned.

## Contributing
Contributions are welcome! Please feel free to submit a pull request or open an issue for any enhancements or bug fixes.

## Acknowledgments
This package utilizes the github.com/google/uuid package for UUID generation.
Special thanks to the Go community for their contributions and support.