## OpenAPI vs Swagger: Key Points for Students

### Big picture

- **OpenAPI (OAS)**: An open, language‑agnostic specification for describing HTTP APIs. It is the contract format (JSON or YAML).
- **Swagger**: A set of tools that understand OpenAPI.
  - **Swagger UI**: renders interactive docs from an OpenAPI file.
  - **Swagger Editor**: in‑browser editor/preview for OpenAPI.
  - **Code generators** (Swagger Codegen, OpenAPI Generator): produce server stubs and SDKs from the spec.

### OpenAPI vs Swagger: Quick comparison

- **OpenAPI**

  - What it is: The open specification/standard for describing HTTP APIs.
  - Purpose: Define the contract (endpoints, inputs, outputs, auth) in JSON/YAML.
  - Ownership: Vendor‑neutral, open governance.
  - Files: Typically `openapi.yaml` or `openapi.json`.
  - Used by: Any tool that reads/writes OpenAPI.

- **Swagger**
  - What it is: A set of tools that work with OpenAPI.
  - Purpose: View, edit, test, and generate code from OpenAPI docs.
  - Includes: Swagger UI, Swagger Editor, Swagger Codegen (and community OpenAPI Generator).
  - Files: Consume the OpenAPI document you provide.
  - Key idea: Swagger implements; OpenAPI defines.

### Why it matters

- **Single source of truth**: one file drives docs, SDKs, mocks, and tests.
- **Contract‑first development**: teams agree on API behavior before coding.
- **Human + machine readable**: easy to read YAML, consumable by tooling.

### Anatomy of an OpenAPI document

- **openapi**: spec version (e.g., `3.0.3`, `3.1.0`).
- **info**: title, version, description, contact, license.
- **servers**: base URLs where the API is hosted.
- **paths**: resources/URIs; each contains HTTP method operations (`get`, `post`, `put`, `delete`, ...).
- **operation fields**: `summary`, `description`, `operationId`, `tags`.
- **parameters**: inputs from `path`, `query`, `header`, or `cookie`.
- **requestBody**: payload definition for methods like POST/PUT/PATCH.
- **responses**: required; map status codes to descriptions and payload schemas.
- **components**: reusable parts: `schemas`, `parameters`, `responses`, `requestBodies`, `headers`, `securitySchemes`.
- **security**: authentication/authorization schemes and application of them.
- **tags**: group operations in the UI (e.g., `Catalog`, `Orders`).
- **externalDocs**: links to guides, tutorials, or extended docs.

### Mental model: HTTP → OpenAPI mapping

- **Resource** → `paths` (e.g., `/products`)
- **Verb/Action** → operation under a path (`get`, `post`, ...)
- **Input** → `parameters` + `requestBody`
- **Output** → `responses` (status codes + content schemas)
- **Reuse/DRY** → `components` referenced with `$ref`

### Minimal, valid example (OpenAPI 3.0.3)

```yaml
openapi: 3.0.3
info:
  title: Shopping API
  version: 1.0.0
servers:
  - url: https://api.example.com/v1
paths:
  /products:
    post:
      tags: [Catalog]
      summary: Add product to catalog
      description: Adds a new product to the store's catalog.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Product"
            examples:
              sample:
                value:
                  name: The Design of Web APIs
                  price: 44.99
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product"
components:
  schemas:
    Product:
      type: object
      required: [name, price]
      properties:
        id:
          type: string
          readOnly: true
        name:
          type: string
        price:
          type: number
          format: float
          minimum: 0
```
