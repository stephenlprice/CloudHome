# Sample Rules

A collection of noteworthy Spectral rules.

### Formats

This style guide validates OpenAPI v2 and v3 as well as json schema loose for models  written with Studio.

``` yaml
formats:
  - oas2
  - oas3
  - json-schema-loose
```
### Extends

The style guide is extending the core OpenAPI ruleset provided by Spectral.

``` yaml
extends:
  - 'spectral:oas'
  - example-spectral-ruleset
```
### Operation Summary <= 20 characters

``` yaml
  operation-short-summary:
    description: 'Operation summary should be short and sweet, no full stops, and less than 20 characters'
    recommended: true
    type: style
    given: '$.paths.*[?( @property === ''get'' || @property === ''put'' || @property === ''post'' || @property === ''delete'' || @property === ''options'' || @property === ''head'' || @property === ''patch'' || @property === ''trace'' )]'
    then:
      - field: summary
        function: pattern
        functionOptions:
          notMatch: \.
      - field: summary
        function: length
        functionOptions:
          max: 20
```

### Operation ID's -> Kebab Case

``` yaml
rules:
  operationIds-kebab-case:
    description: Operation IDs MUST be written in kebab-case
    message: '{{property}} is not kebab-case: {{error}}'
    recommended: true
    type: style
    given: '$.paths.*[?( @property === ''get'' || @property === ''put'' || @property === ''post'' || @property === ''delete'' || @property === ''options'' || @property === ''head'' || @property === ''patch'' || @property === ''trace'' )]'
    then:
      field: operationId
      function: pattern
      functionOptions:
        match: '^([a-z0-9-]+)$'
```
### GET requests do not accept Body parameters

``` yaml
  request-GET-no-body:
      description: A `GET` request MUST NOT accept a `body` parameter
      severity: error
      recommended: true
      given: $.paths..get.parameters..in
      then:
        function: pattern
        functionOptions:
          notMatch: /^body$/
```

### OAS3 only HTTPS

``` yaml
  oas3-hosts-https-only:
    description: ALL requests MUST go through `https` protocol only
    formats:
      - oas3
    recommended: true
    severity: error
    message: Servers MUST be https and no other protocol is allowed.
    given: $.servers..url
    then:
      function: pattern
      functionOptions:
        match: '/^https:/'
```
### Schema properties -> Camel Case and ASCII Alphanumeric

``` yaml
  properties-camelCase-alphanumeric:
    description: All JSON Schema properties MUST follow fields-camelCase and be ASCII alphanumeric characters or `_` or `$`.
    severity: error
    recommended: true
    message: '{{property}} MUST follow camelCase and be ASCII alphanumeric characters or `_` or `$`.'
    given: '$.definitions..properties[*]~'
    then:
      function: pattern
      functionOptions:
        match: '/^[a-z$_]{1}[A-Z09$_]*/'
```

### Configuring Severity for Extended Rulesets

``` yaml
  operation-singular-tag: hint
  tag-description: hint
  operation-default-response: info
  openapi-tags-alphabetical: hint
  oas3-server-not-example.com: warn
  oas3-parameter-description: error
  oas2-parameter-description: error
  oas2-host-not-example: warn
  info-license: info
  license-url: info
  contact-properties: info

```
