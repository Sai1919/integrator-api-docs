# Introduction
The Integrator is an API first platform—whereby every new feature is released first in the API, and then shortly after in the UI.  The API is RESTful, uses JSON, and is secured by Bearer Tokens.  The target audience for the API is developers who wish to build fully featured, fully managed integration based apps.  Complementing the API, the Integrator platform supports an incredibly rich customization framework.  The integrations that you build can include custom installers, custom settings pages, along with any number of custom integration hooks (integration hooks give you the ability to write your own custom code, and are especially useful for requirements that just can't be implemented via configuration alone).  Any integrations that you build can also be listed in the Integrator Marketplace for easy install by any other Integrator user.  Enjoy!

## Quick Start
## Authentication
### Bearer Tokens
Every Integrator account is
### One Time Tokens
## Rate Limiting
## Headers


# Resources
## Connection
## Export
## Import

## Mapping
The mapping resource is used to describe the data transformation that should take place between a source document (perhaps from an export) and the document to be imported into a destination system.  

Note that a mapping resource is an optional feature of an [Import](#import). By default, if no explicit mapping is defined imports will simply pass along the source document to the destination system.

### Field Mapping
The most basic transformation possible is mapping fields from one name to another.

Consider this sample source document:
```javascript
var contact = {
  first: 'John',
  last: 'Hancock',
  age: '43',
  isLocal: 'true'
}
```
If the destination system required only the first and last names, and used the properties firstName and lastName, the mapping below would accomplish this transformation:

```javascript
{
  name: 'example-mapping-1',
  fieldMapping: [
    {eField: 'first', gField: 'firstName'},
    {eField: 'last', gField: 'lastName'},
  ]
}
```
The mapping above would generate the document below:

```javascript
{
  firstName: 'John',
  lastName: 'Hancock'
}
```
Note that if an [Import](#import) has a mapping defined, only the fields explicitly defined in the mapping will be included in the document passed to the destination system.

The simple example above can be enhanced in several ways. First, the values of the _eField_ and _gField_ respect JSON dot-notation. This allows the mapping to traverse the source document when extracting values, and likewise generate a complex document.  It is also possible to add hardcoded fields to the generated document.  Finally, simple data type conversions are available. Supported types are _string, number, boolean_. The example below demonstrates these additional features:

```javascript
var mapping = {
  name: 'example-mapping-2',
  fieldMapping: [
    {eField: 'first', gField: 'contact.firstName'},
    {eField: 'last', gField: 'contact.lastName'},
    {eField: 'age', gField: 'contact.age', dataType: 'number'},
    {eField: 'isLocal', gField: 'isLocal', dataType: 'boolean'},
    {gField: 'isNew', hardCodedValue: true}
  ]
}

var generatedDocument = {
  isNew: true,
  isLocal: true,
  contact: {
    firstName: 'John',
    lastName: 'Hancock',
    age: 43
  }
}
```
Note that when type conversion is specified, the following rules apply:
* boolean: null, undefined, false, NaN, 'false', 'off', and 'no' resolve to false. All other values resolve to true.
* number: null, undefined, NaN, and any non-numeric string value resolve to 0.
* string: simply the string equivalent of the value.

There is one final field mapping feature. The use of simple expressions as the eField value. Using eFiedl expression is a way to combine any number of source fields into a single field in the generated document.

The example below will demonstrate the use and syntax of an eField expression:
```javascript
var issue = {
  id: 10002,
  name: 'unable to login',
  message: 'User forgot his password. Their password has been reset.',
  userId: 12345
}

var mapping = {
  name: 'example-mapping-3',
  fieldMapping: [
    {eField: 'Issue-{id}: *{name}*  {message}', gField: 'message'},
    {eField: 'first', gField: 'contact.firstName'},
  ]
}

var generatedDocument = {
  message: 'Issue-10002: *unable to login*  User forgot his password. Their password has been reset.'
}
```
## Flow
## Integration
## Connector
## IClient
## Job
## Job Error
## Retry
