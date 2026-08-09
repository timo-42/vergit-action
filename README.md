# vergit-action

Generate a [PEP 440](https://peps.python.org/pep-0440/) version string from
Git state with [vergit](https://github.com/timo-42/vergit).

```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 0

- uses: timo-42/vergit-action@v1
  id: version

- run: echo "building ${{ steps.version.outputs.version }}"
```

`fetch-depth: 0` is important: shallow checkouts hide tags, which prevents
vergit from deriving the correct version.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `tool-version` | `latest` | vergit release tag to download |
| `tool-repository` | `timo-42/vergit` | Repository containing vergit releases |
| `tag-prefix` | `v` | Prefix to strip from version tags |
| `no-local` | `false` | Omit the PEP 440 local version label |
| `next` | empty | Next release level: `major`, `minor`, or `patch` |
| `next-phase` | empty | Pre-release phase: `alpha`, `beta`, or `rc` |
| `with-prefix` | `false` | Prepend the tag prefix to next-version output |
| `fetch-tags` | `true` | Fetch tags and unshallow the checkout when needed |
| `working-directory` | `.` | Directory whose Git state to inspect |

The action exposes its value as the `version` output.
