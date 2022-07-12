# Click Farmer

•	A web application that keeps track of how many clicks you give between multiple choices. 
•      Uses Go for the backend and JavaScript for the API.
•      Adopts web responsive design.
•	Built a webserver that maintains a local cache of click counts and periodically syncs these values with the database, to precisely collect clicks with low load.

## The Problem

The click farming business has really taken off recently.

The original click farming engineers started this project to count the number of
clicks around the world.

The goal is to get the product working to farm upvotes from around the world.


## Building and Running

To build the binary:
```
go build -o clickfarmer -race -v .
```

To run the database:
```
./clickfarmer database
```
You can specify an `--rpc-addr <addr>` flag to set the RPC server address the
database listens for requests on to something other than ":8080".

To run a webserver:
```
./clickfarmer webserver
```
You can specify an `--rpc-addr <addr>` flag to set the address to connect to
the database on (it defaults to "localhost:8080").

You can specify an `--http-addr <addr>` (or `-a`) flag to set the address to
serve the website on. This is used to run multiple web servers, like:

```
./clickfarmer webserver -a :3001
./clickfarmer webserver -a :3002
./clickfarmer webserver -a :3003
```

You can also run the database and webserver without the `build` step
by running the `main.go` file directly:

```
go run main.go database
go run main.go webserver -a :3002
```

To access the webserver's frontend, go to http://localhost:3000 in a browser (if
you used `--http-addr` or `-a` to specify a different one, use that port
instead of `3000`).

## Generating Protobufs

Install the protobuf compiler:

https://grpc.io/docs/protoc-installation/

As well as some additional dependencies:

```
go get google.golang.org/protobuf/cmd/protoc-gen-go \
         google.golang.org/grpc/cmd/protoc-gen-go-grpc
```

To re-generate the protobuf code after changing `pb/clicktracking.proto`, run:
```
go generate ./pb/...
```

## Api

```
GET /api/clicks/red -> returns a single integer representing total red button clicks (`0`)
GET /api/clicks/green -> returns a single integer representing total green button clicks
GET /api/clicks/blue -> returns a single integer representing total blue button clicks
GET /api/clicks -> returns JSON like `{"redClicks": 0, "greenClicks": 0, "blueClicks": 0}`
PUT /api/clicks/red -> called when the red button was clicked once. no return data
PUT /api/clicks/green -> called when the green button was clicked once. no return data
PUT /api/clicks/blue -> called when the blue button was clicked once. no return data
```


