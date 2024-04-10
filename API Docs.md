To achieve re-use of capability and share data, self-service discoverability is encouraged, it is the responsibility of the system providing the contract to ensure it is well-defined, i.e. specification of the API's endpoints, I/O payload and the schema.

## airtasker/spot v.s. Swagger
### Spot
- TS as the foundation to define API contracts
- Contract-first so as to ensure the API design is solidified before implementation begins
- Generate OpenAPI (Swagger) specifications from the TS API contracts
- a more concise and type-safe way to define APIs

### Swagger
- **YAML or JSON Specifications**
- Wide tooling support: includes tools for automatically generating docs (Swagger UI), client and server code (Swagger Codegen), and for testing and validation
- Language-agnostic: Unlike Spot, which is TS-based, Swagger specifications are language-agnostic, making it universally applicable across different programming languages and environments, broadening its usability.

**How should they be used?**
They fundamentally serve the same purpose: to describe and document APIs, so use the right tools for different stages and aspects of the API lifecycle to maximize efficiency.
- Use Spot to define API contracts directly in TS for type safety and potential code reuse.
- Generating an OpenAPI specification from Spot definitions, then use Swagger's rich tools: Swagger UI, Swagger Editor.
- Automate the generation of the OpenAPI spec from Spot as part of the build process in CI/CD pipeline.
- Update README.md


Spot, by design, focuses on API type safety and may not directly support all the specific validation rules (like minLength for a string) or detailed security definitions that OpenAPI supports out of the box. However, you can employ certain strategies to enrich Spot definitions, ensuring the generated Swagger documentation reflects security and validation requirements as closely as possible.

What steps am I missing in this process?

What does schemaGen do?


Turnstile
A side-car along OLAPI to do authentication and authorization by ABAS(Attributes-Based Access Control). For an API doc, how should I embody this, i.e. the endpoints requires attributes validation?

Authentication verifies the identity of a user or service, and authorization determines their access rights.


What should the validation rules be like in the swagger yml?
What should Spot do to auto generate the validation rules?

**Generate Swagger YAML**
- Generate `Swagger YAML`(sy) based on the API contracts defined by Spot and put it in the right place
- Attach necessary contents, mainly validation check to `sy`. Also components like servers.
	- Some can go with the schemes definition provided by Swagger 
	- while more complex condition can only be captured by comments
		- interaction with turnstile configuration files
	=> so a generation command for Swagger YAML will generate schema based on:
	- API contracts defined by Spot
	- Validation rules defined by turnstile files


Solution:
	1. Typing can do some validation like non-empty string. For the other validations, we can consider adding @description or comments to the API contracts defined by Spot. This could be an inline section or separate documentations that will include:
		- attributes used
		- conditions and effects
		- examples of payloads
		- using diagrams if things get too complex
	2. Skip Spot and directly define the validation rules in Swagger yml that provides some validation entries
		- Pattern, Enum, Min/Max Length, etc. and custom validations.
		- Or Security Schemes and Requirements.



Validate and deploy the schema => Swagger Codegen: generate client libraries or server stubs so as to bridge between design and implementation
- test?