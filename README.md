
# Simple Cached Fileserver

[![Build Status](https://travis-ci.org/natebrennand/cached_fileserver.svg)](https://travis-ci.org/natebrennand/cached_fileserver)


## Building
`Make` will generate both "tcp_server" and "tcp_client" binaries in the "go" directory.




## Client
The client is implement exactly as specified.
A newline is sent to signify the end of the filename.
There is an implicit assumption made by the client that when no data is sent by the server before closing the connection that the file was not found.
There will be no data returned for invalid requests that ask for data outside the served directory.




## Server
Requests for files outside of the served directory are not allowed.
This is handled by checking for filepaths starting with "../" and "/*".
The path is normalized by the "path" package beforehand.

The server was tested with filesizes up to 12 GB successfully.




## Cache System
Members of the cache are not evicted in the case that the file is > 64 MB.
In the case that adding the file would push the size of the cache over 64MB, the last used files are evicted from the cache until there is sufficient space.

The reading of the files is done by the cache when it does not find the file in the cache.
All specified log messages are implemented with the addition of one for when the queried file is too large to enter the cache.

The pointers to byte arrays in the cache are not counted in the size of the cache.




## Tests
There are some tests to prove the cache's functionality.
They can be invoked by running `go test` in the "server" directory or `go test ./...` from the "go" directory.




## Go Version
Tested with Go 1.1 and 1.3 successfully.


