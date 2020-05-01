# Overview

This repository contains a set of mustache templates which are meant to work with [OpenAPI Generator](https://openapi-generator.tech/). These
templates take the existing templates and adds vendor extensions to the OpenAPI spec to support arbitrary annotations on a class,
field, getter, and/or setter. For example, in an OpenAPI Specification where you define a Schema type:

```yaml
    Employee:
      type: object
      x-java-class-annotations:
        - "@javax.persistence.Entity"
        - |-
            @javax.persistence.Table(
              name = "employees",
              uniqueConstraints = {
                @javax.persistence.UniqueConstraint(columnNames = {"email"})
              }
            )
      properties:
        email:
          description: The email address of the employee.
          type: string
          example: hgranger@redhat.com
          x-java-field-annotations:
            - "@javax.persistence.Id"
        name:
          description: Name of the employee.
          type: string
        role:
          description: "The role of the employee. For example, consultant, PM, TSM, etc."
          type: string
          example: Consultant
```

The `x-java-class-annotations` and the `x-java-field-annotations` would be integrated into the model for `Employee` so that it would be compatible with JPA.


## Supported Extensions To OpenAPI

* `x-java-class-annotations`: Can be used on any Schema Type definition which is not just a reference to another type
  * ```yaml
    Employee:
      type: object
      x-java-class-annotations:
        - "@javax.persistence.Entity"
        - |-
            @javax.persistence.Table(
              name = "employees",
              uniqueConstraints = {
                @javax.persistence.UniqueConstraint(columnNames = {"email"})
              }
            )
      properties:
    ```
  * `x-java-class-annotations`: Can be used on any property of a Schema Type which is not just a reference to another type
    ```yaml
      properties:
        email:
          description: The email address of the employee.
          type: string
          example: hgranger@redhat.com
          x-java-field-annotations:
            - "@javax.persistence.Id"
        skill:
          type: object
          x-java-field-annotations:
          - "@javax.persistence.ManyToOne(targetEntity = Skill.class)"
          - "@javax.persistence.JoinColumn(name = \"skill_id\", insertable = false, updatable = false)"
          allOf:
            - $ref: '#/components/schemas/Skill'
    ```
  * `x-java-getter-annotations`: Can be used on any property of a Schema Type which is not just a reference to another type
  * `x-java-setter-annotations`: Can be used on any property of a Schema Type which is not just a reference to another type

## Usage

1. Clone this repository into a directory
2. Install [OpenAPI Generator](https://openapi-generator.tech/docs/installation)
3. Run `openapi-generator` with some additional parameters like `-t /path/to/templates`
