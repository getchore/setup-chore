# setup-chore

Installs the [chore](https://github.com/getchore/chore) task runner on a GitHub
Actions runner and puts it on `PATH`. Works on `ubuntu-*`, `macos-*` and
`windows-*` runners, x86-64 and arm64.

```yaml
- uses: getchore/setup-chore@v1
- run: chore build
```

Pin a release instead of tracking the latest:

```yaml
- uses: getchore/setup-chore@v1
  with:
    version: v1.4.1
```

## Inputs

| name | default | description |
| --- | --- | --- |
| `version` | latest release | Release tag to install. A leading `v` is optional. |

## Outputs

| name | description |
| --- | --- |
| `version` | What `chore --version` reports after installing. |

## Why this exists

chore's whole point is that a task behaves the same on every OS, so a workflow
that uses it should not need a matrix leg per platform:

```yaml
jobs:
  build:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: getchore/setup-chore@v1
      - run: chore build
      - run: chore test
```

The one `runner.os` branch chore cannot remove is the one that installs chore
itself — curl and PowerShell are not the same program. That branch lives in
this action, once, instead of in every workflow that wants the tool.

## License

MIT
