<!--
SPDX-FileCopyrightText: 2022 Risk.Ident GmbH <contact@riskident.com>

SPDX-License-Identifier: CC-BY-4.0
-->

# gocrud

[![REUSE status](https://api.reuse.software/badge/github.com/RiskIdent/gocrud)](https://api.reuse.software/info/github.com/RiskIdent/gocrud)

Simple CRUD application that exposes an HTTP REST API to store data inside
a MongoDB database.

## Usage

### Configuration

gocrud is configured via command-line flags or environment variables.

| Flag             | Environment variable  | Default                     | Description             |
| ---------------- | --------------------- | --------------------------- | ----------------------- |
| `--bind-address` | `GOCRUD_BIND_ADDRESS` | `0.0.0.0:8080`              | Address to serve API on |
| `--mongo-uri`    | `GOCRUD_MONGO_URI`    | `mongodb://localhost:27017` | MongoDB URI to use      |
| `--mongo-db`     | `GOCRUD_MONGO_DB`     | `gocrud`                    | MongoDB database to use |

### MongoDB authentication

Authentication can be provided via the MongoDB URI. Example:

```properties
GOCRUD_MONGO_URI=mongodb://admin:password@localhost:27017
```

## API

By default, gocrud exposes the following endpoints on port 8080:

- [`POST /v1/server` Create server](#create-server)
- [`GET /v1/server` Get server](#get-server)

### Create server

```http
POST /v1/server
```

Creates a new server, and returns the ID of the server created.

Request body:

```json
{
  "name": "string",
  "description": "string",
  "datacenter": "string"
}
```

Responses:

> Status: **200 OK**\
> Body:
>
> ```json
> {
>   "id": "string"
> }
> ```

> Status: **400 Bad Request**\
> Body:
>
> ```json
> {
>   "error": "string"
> }
> ```

> Status: **500 Internal Server Error**\
> Body:
>
> ```json
> {
>   "error": "string"
> }
> ```

### Get server

```http
GET /v1/server/:id
```

Retrieves an existing server.

Parameters:

- `:id` *(path)*: ID of the server object.

Responses:

> Status: **200 OK**\
> Body:
>
> ```json
> {
>   "id": "string",
>   "name": "string",
>   "description": "string",
>   "datacenter": "string"
> }
> ```

> Status: **400 Bad Request**\
> Body:
>
> ```json
> {
>   "error": "string"
> }
> ```

> Status: **404 Not Found**\
> Body:
>
> ```json
> {
>   "error": "string"
> }
> ```

> Status: **500 Internal Server Error**\
> Body:
>
> ```json
> {
>   "error": "string"
> }
> ```

## Development

### Prerequisites

- Go 1.19 (or higher)
- A way to run MongoDB locally, e.g via a container using [Podman](https://podman.io/)

### Running locally

1. Start up a local MongoDB instance, for example via [Podman](https://podman.io/):

   ```sh
   podman run --rm -it -p 27017:27017 mongo
   ```

2. Run gocrud locally, e.g:

   ```bash
   go run .
   ```

3. To test out the webhooks, you can make use of our example webhook like so:

   ```console
   $ curl localhost:8080/v1/server -d @examples/server.json
   {"result":{"InsertedID":"63d00f3a87cb268ed07657e6"}}

   $ curl localhost:8080/v1/server/63d00f3a87cb268ed07657e6
   {"result":{"name":"main","description":"The main server hosted in central Europe, right at the bottom of the baltic lake in a hidden underwater base.","datacenter":"eu_central_1"}}
   ```

## License

This repository complies with the [REUSE recommendations](https://reuse.software/).

Different licenses are used for different files. In general:

- Go code is licensed under GNU General Public License v3.0 or later ([LICENSES/GPL-3.0-or-later.txt](LICENSES/GPL-3.0-or-later.txt)).
- Documentation licensed under Creative Commons Attribution 4.0 International ([LICENSES/CC-BY-4.0.txt](LICENSES/CC-BY-4.0.txt)).
- Miscellaneous files, e.g `.gitignore`, are licensed under CC0 1.0 Universal ([LICENSES/CC0-1.0.txt](LICENSES/CC0-1.0.txt)).

Please see each file's header or accompanied `.license` file for specifics.
