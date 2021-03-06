# Evenstar
Just another GraphQL query builder, but composable!

## How to install it?
It's simple, just run: `pip install evenstar`

## GraphQL Syntax Coverage
- [X] Fields
- [X] Arguments
- [X] Aliases
- [ ] Fragments
- [X] Operation Name
- [X] Variables
- [ ] Directives
- [X] Mutations
- [X] Inline Fragments

## Example

```python
apple_field = Field(
    name="appleMutation",
    alias="apple_mutation_alias",
    arguments=Arguments(
        {"input": VarSymbol("$apple_input")}
    ),
    children=[
        "__typename",
        InlineFragment(
            on="SomeErrKind",
            children=["code", "field", "message"]
        ),
        InlineFragment(
            on="SomeAnotherErrKind",
            children=["error_code"]
        ),
    ],
)

create_banana_field = Field(
    name="createBanana",
    alias="create_banana",
    arguments=Arguments(
        {"input": VarSymbol("$create_banana_input")}
    ),
    children=[
        "__typename",
        "code",
        InlineFragment(
            on="SomeAnotherErrKind",
            children=["error_code"]
        ),
        InlineFragment(
            on="Banana",
            children=[
                "id",
                "quality",
                Field(
                    "farm",
                    children=[
                        "country",
                        "town",
                        "number",
                    ],
                ),
            ],
        ),
    ],
)

gql_req = Request(
    children=[
        Mutation(
            children=[create_banana_field, apple_field],
            variables=[
                VarDeclaration("$apple_input", "CreateAppleInput!"),
                VarDeclaration("$banana_input", "CreateBananaInput!"),
            ],
        )
    ],
    variables={
        "apple_input": {"id": str(uuid4())},
        "banana_input": {"id": str(uuid4())},
    },
)

print(gql_req.json(indent=2))
```
