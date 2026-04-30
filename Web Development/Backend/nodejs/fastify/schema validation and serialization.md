Tại sao fastify lại sử dụng schema validation
sử dụng type box để khai báo schema.
và dùng jva là engine để validate schema

lý do nên sử dụng

ref đến schema đã tồn tại trước đó

các conditional logic có thể áp dụng được cho schema validation bao gồm:
- check field có thuộc 1 trong type trong `json schema` 
- number check dc max, min, multiple
- object type ta kiểm tra property name và type


- **JSON Schema Compliance**: TypeBox generates standard JSON Schema objects, which use the `$ref` keyword to reference other schemas (often stored in the `$defs` section).
- **Static Type Inference**: Using `Type.Ref()` allows TypeScript to correctly infer the static types (`Static<typeof ...>`) of the properties that use the reference, ensuring type safety throughout your application.
- **Circular References**: TypeBox also provides utilities like `Type.Recursive()` and `Type.Deref()` to handle more complex scenarios such as circular dependencies between schemas.

merging schema

create reusable schema 

đảm bảo support typing và auto-completion 
support các cú pháp và typing quy định trong JSON schema như object extension hoặc object reference

dễ dàng thao tác 


json schema với ajv 
khai báo type cho schema trước và sử dụng `JSONSchemaType`

note: sử dụng `JSONSchema7` không hint type được cho fastify

để fastify nhận diện được typing ta có thể sử dụng `as const` hoặc JSON

```typescript
type UpdateTaskSchema = {
	part1: string
	part2: string
}

// Fail. fastify can not infer typing from this declaration
export const updateTaskSchema:JSONSchemaType<UpdateTaskSchema> = {
	type: 'object',
	properties: {
		part1: { type: 'string' },
		part2: { type: 'string' },
	},
	required: ['part1'],
}

// OK
export const updateTaskSchema = {
	type: 'object',
	properties: {
		part1: { type: 'string' },
		part2: { type: 'string' },
	},
	required: ['part1'],
} satisfies JSONSchemaType<UpdateTaskSchema>
```


reference other typing 

following [this approach ](https://fastify.dev/docs/latest/Reference/Validation-and-Serialization/#json-schema-support) may help optimize schema validation a little bit how ever it would loose the benefit of type safety and convenient auto-completion. However, you assign existing schema as the reference when defining new schema.

```typescript
type GetTaskSchema = {
	id: string
}
export const getTaskSchema = {
	type: 'object',
	properties: {
		id: { type: 'string', format: 'uuid' },
	},
	
	required: ['id'],
} satisfies JSONSchemaType<GetTaskSchema>


type UpdateTaskSchema = {
	ref: GetTaskSchema
	part1: string
	part2: string
}


export const updateTaskSchema = {
	type: 'object',
	properties: {
		ref: getTaskSchema,
		part1: { type: 'string' },
		part2: { type: 'string' },
	},
	required: ['part1'],
}
```

``` json
{
  coerceTypes: 'array', // change data type of data to match type keyword
  useDefaults: true, // replace missing properties and items with the values from corresponding default keyword
  removeAdditional: true, // remove additional properties if additionalProperties is set to false, see: https://ajv.js.org/guide/modifying-data.html#removing-additional-properties
  uriResolver: require('fast-uri'),
  addUsedSchema: false,
  // Explicitly set allErrors to `false`.
  // When set to `true`, a DoS attack is possible.
  allErrors: false
}

```

extend from other object schema


error  handling


terrible typing compatible 

authentication
