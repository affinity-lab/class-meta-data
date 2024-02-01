# ClassMetaData

The ClassMetaData library provides a powerful mechanism for attaching metadata to classes in TypeScript. This metadata can be useful for various purposes such as configuration, annotation, or any other application-specific data associated with classes.

## Writing the metadata

### 1. Create an Instance of ClassMetaData:

Begin by creating an instance of the `ClassMetaData` class. This instance will be used to manage and organize metadata
stores for various classes.

```ts
const metaData = new ClassMetaData();
```

### 2. Access or Create a MetaDataStore:

Use the `get` method to access or create a `MetaDataStore` for the target class. If the store doesn't exist and you want
to create it, set the create parameter to `true`.

```ts
// Example class
class ExampleClass {
	// ... class implementation ...
}

// Access or create metadata store for ExampleClass
const metaDataStore = metaData.get(ExampleClass, true);
```

### 3. Write Metadata with MetaDataStore:

Once you have the `MetaDataStore` instance, you can use its methods to write metadata. The `MetaDataStore` provides the following methods:

> `merge(key: string | string[], value: Record<string, any>): void`
>
> Merge metadata with existing data.

> `set(key: string | string[], value: any): void`
>
> Set a single value for the specified `key`.

> `push(key: string | string[], value: any): void`
>
> Push a value to an array associated with the `key`.

> `delete(key: string | string[]): void`
>
> Delete metadata associated with the specified `key`.

```ts
// Set a single metadata value
metaDataStore?.set('propertyName', 'propertyValue');
```

#### Data types

The data manipulation methods: `set`, `push`, and `merge` are provided by the ClassMetaData library for writing metadata. These methods play a pivotal role in shaping the structure of metadata associated with TypeScript classes, offering flexibility in handling single values, arrays, and complex nested properties.

##### 1. set: Setting a Single Value
The `set` method in the `ClassMetaData` library is used to set a single value for a specific key or property. This method is suitable when you want to assign a straightforward, standalone value to a metadata property. It overwrites any existing data associated with the specified key, ensuring that only the most recent value is retained.

```ts
// Setting a single value for a metadata property
metaDataStore.set('propertyName', 'propertyValue');
```

##### 2. push: Adding to an Array
The `push` method is employed when dealing with properties that hold an array of values. It appends a new value to the existing array associated with the specified key. This is particularly useful when multiple values need to be associated with the same property, creating an array of metadata for a given key.

```ts
// Adding a value to an array property in metadata
metaDataStore.push('arrayProperty', 'arrayValue1');
metaDataStore.push('arrayProperty', 'arrayValue2');
```

##### 3. merge: Merging with Existing Data
The merge method allows for combining new metadata with existing data. It is especially useful when dealing with nested properties or complex metadata structures. The merge operation ensures that the existing metadata is preserved while incorporating the new values. This method is versatile, accommodating various data structures and enabling the creation of intricate metadata hierarchies.

```ts
// Merging metadata with existing data for nested properties
metaDataStore.merge('nested.property.subProperty', 'subValue1');
metaDataStore.merge(['nested', 'property', 'subProperty'], 'subValue2');
```

##### Consistency Note:
It's important to note that maintaining consistency in the choice of method (`set`, `push`, or `merge`) when interacting with the same property or key is crucial. Using the same method ensures a standardized approach, avoiding confusion and enhancing code readability when managing metadata for a particular class. Choose the method that aligns with the nature of the metadata and the desired structure for a more organized and maintainable codebase.

#### Flexible Nesting with Dot Notation or String Arrays

The `ClassMetaData` library provides flexibility in how you define and handle nested properties within your metadata. This section outlines two approaches: using dot notation on the key and passing a string array as the key argument.

##### 1. Dot Notation for Nesting

The dot notation is a concise and expressive way to represent nested properties within your metadata. When using dot notation, each dot represents a level of nesting. The `merge` method is designed to work seamlessly with dot notation, making it intuitive to create and manage nested properties.

```ts
// Using dot notation for nesting
metaDataStore?.merge('nested.property.subProperty', 'subValue');
```

##### 2. String Array as Key Argument
Alternatively, you can use a string array to represent the hierarchy of nested properties. Each element in the array corresponds to a level of nesting. This approach is particularly useful when the nesting structure is dynamic or when the nesting levels are determined at runtime.

```ts
// Using string array as the key argument for nesting
metaDataStore.merge(['nested', 'property', 'subProperty'], 'subValue');
```
## Reading Metadata in ClassMetaData

Once metadata has been set using the `ClassMetaData` library, retrieving and interpreting this information is vital for leveraging the associated values. 

To retrieve metadata for a specific class, use the `read` method of the ClassMetaData class. 

```ts
// Reading metadata for ExampleClass
const metadataResult = metaData.read(ExampleClass);
console.log(metadataResult);
```

The read method's second optional argument is an options object with two properties:

- `flatten` (boolean, default: `false`): If set to `true`, the return will be a "flattened" object, where sub keys are separated by dots (dot-notation) according to their specification. If the value is `false`, the response will maintain a regular object hierarchy.

- `simplify` (boolean, default: `true`): If set to `true`, the response will include only the most relevant data based on inheritance rules. If set to `false`, the response will include all data generated during the inheritance process.

```ts
// Reading metadata for ExampleClass
const metadataResult = metaData.read(ExampleClass, {flatten: false, simplify: true});
console.log(metadataResult);
```

### Results with inheritance (`{simplify:false}`)

If the examined object is part of an inheritance chain, the system also unfolds the metadata generated during inheritance.

When the `simplify` option is set to `false`, the return of the read method will be an `MetaValue` object with the following properties:

- `value`: The most recently set value during inheritance.
- `self`: The value present in the requested class.
- `inherited`: An array containing values set in the order of the inheritance chain.

This structure provides comprehensive information about the values associated with the class, both inherited and self-defined.