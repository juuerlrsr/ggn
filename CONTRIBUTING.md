# Commenting on RFC GGN-1

GGN is a draft, and comments shape the final version. Anyone can take part.

## Comments and questions

[Open an issue](../../issues/new/choose) for anything about the spec: something unclear, a case
it doesn't cover, a problem with a rule, or a different idea. Please say which part and section
you mean, and include a short example record if it helps.

## Proposing a change

For a concrete change to the text, open a pull request that edits [`spec/ggn.md`](spec/ggn.md).
Describe what changes and why, and whether existing records would still read the same way.

- **Wording and examples:** fixes that don't change what a record means.
- **Additions:** a new optional tag, annotation or extension. Existing records stay valid.
- **Incompatible changes:** anything that changes how existing records are read. Allowed while
  GGN is a draft; after 1.0.0 they need a new major version.

Requirement words (**MUST**, **SHOULD**, **MAY** …) follow [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119)
and [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174): use them only in capitals, only in the
normative parts (Parts 1 and 2), and only for real requirements.

## License of contributions

The spec is dedicated to the public domain under [CC0 1.0](LICENSE). By contributing, you agree
that your contribution is dedicated the same way.
