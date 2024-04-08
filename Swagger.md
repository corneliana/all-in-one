Goal:
- Specification of the API's endpoints, I/O payload and the schema
- Validate and deploy the schema => Swagger Codegen: generate client libraries or server stubs so as to bridge between design and implementation
- test?

An API instruction <= yaml / json file containing the RESTful endpoints and schema
- handwritten? No!
- can it be generated based on the routes?
- how can I use it? Just drop it in the https://editor.swagger.io/? Any other ways?

Swagger 

schemaGen


Swagger v.s. airtasker/spot
They fundamentally serve the same purpose: to describe and document APIs.

- airtasker/spot
	- TS as the foundation to define API contracts
	- A contract-first approach to ensure that the API design is solidified before implementation begins
	- Generate OpenAPI (Swagger) specifications from the TypeScript API contracts
	- a more concise and type-safe way to define APIs
- Swagger
	- **YAML or JSON Specifications**
	- Wide tooling support: includes tools for automatically generating documentation (Swagger UI), client and server code (Swagger Codegen), and for testing and validation
	- Language-agnostic: Unlike Spot, which is TS-based, Swagger specifications are language-agnostic, making it universally applicable across different programming languages and environments, broadening its usability.

Should we use a language-specific approach?
- TypeScript for Spot and YAML/JSON for Swagger. Spot is more developer-friendly while Swagger is more flexible with more established ecosystem.

Combine them - use the right tools for different stages and aspects of the API lifecycle to maximize efficiency, collaboration, and developer satisfaction.
- Use Spot to define API contracts directly in TS, esp useful if we are already using TS, which provides type safety and potential code reuse.
- Generating an OpenAPI specification from Spot definitions, then we can use Swagger's rich tools: Swagger UI, Swagger Editor.
- Automate the generation of the OpenAPI spec from Spot as part of the build process in CI/CD pipeline.

Authentication & Authorization
