# vergit-action

Generate a [PEP 440](https://peps.python.org/pep-0440/) version string from
Git state with [vergit](https://github.com/timo-42/vergit).

```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 0

- uses: timo-42/vergit-action@main
  id: version

- run: echo "building ${{ steps.version.outputs.version }}"
```

`fetch-depth: 0` is important: shallow checkouts hide tags, which prevents
vergit from deriving the correct version.

The action currently defaults to `vergit` `v0.0.1b1`. Override
`tool-version` to use another compatible release:

```yaml
- uses: timo-42/vergit-action@main
  with:
    tool-version: v0.0.1b1
```

## Gitea Actions

The same composite action works on Gitea. Pin a release and point
`tool-server` at the server hosting the `vergit` release assets:

```yaml
- uses: https://github.com/timo-42/vergit-action@main
  id: version
  with:
    tool-version: v0.1.0
    tool-server: https://github.com
```

For a self-hosted release, set `tool-repository` and `tool-server` to that
Gitea repository and instance. Pinning is recommended because not every Gitea
installation supports the `releases/latest/download` endpoint.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `tool-version` | `v0.0.1b1` | vergit release tag to download, or `latest` |
| `tool-repository` | `timo-42/vergit` | Repository containing vergit releases |
| `tool-server` | `https://github.com` | Server hosting the vergit release assets |
| `tag-prefix` | `v` | Prefix to strip from version tags |
| `no-local` | `false` | Omit the PEP 440 local version label |
| `next` | empty | Next release level: `major`, `minor`, or `patch` |
| `next-phase` | empty | Pre-release phase: `alpha`, `beta`, or `rc` |
| `with-prefix` | `false` | Prepend the tag prefix to next-version output |
| `fetch-tags` | `true` | Fetch tags and unshallow the checkout when needed |
| `working-directory` | `.` | Directory whose Git state to inspect |

The action exposes its value as the `version` output.
