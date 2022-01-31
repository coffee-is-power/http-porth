# HTTP-PORTH

This is a simple http library for the [porth programming language](https://gitlab.com/tsoding/porth)
Example:
```

include "std.porth"
include "http.porth"

memory count sizeof(u64) end

proc handle_request
  int ptr // Route
  int // Client File Descriptor 
in 
  let route_size route fd in
  route_size route "/" streq if
    count inc64
    fd HTTP_OK // Sends an OK request
    "Hello World<br>" fd fputs
    "This page was accessed " fd fputs
    count @64 fd fputu
    " times.\r\n" fd fputs
    else
      fd HTTP_NOT_FOUND // Sends a 404 code
    end
  end
end
proc main in
  addr-of handle_request // Handle Request function pointer for the listen procedure
  0 0 0 0 // IP Address to bind to
  3000 // Port
  http::create_server // this returns the socket file descriptor
  http::listen // Starts listening to connections
end
```
You can also serve static files if you need to:

main.porth:
```
.......

proc handle_request
  int ptr // Route
  int // Client File Descriptor 
in 
  let route_size route fd in
    route_size route fd "./public"c http::static
  end
end

.....
```
public/index.html:
```html
<h1>Hello from porth!</h1>
```