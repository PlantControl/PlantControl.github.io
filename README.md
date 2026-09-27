# PlantControl domain

This GitHub Pages site serves Go import metadata, each pointing at
`https://github.com/PlantControl/<repo>`:

- Public modules: `plantcontrol.org/v1/<repo>` (`gonum`, `controlsys`).
- Private modules: `plantcontrol.org/private/<repo>` (`systemid`, `process-lab`).
  Consumers set `GOPRIVATE=plantcontrol.org/private` and need Git access.

To add a module, create `v1/<repo>/index.html` or `private/<repo>/index.html`
and add its `go-import` tag to `404.html`.

The custom domain is `plantcontrol.org`. At Spaceship, configure four apex
`A` records (`@`) for GitHub Pages:

- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

The site is served from the root of `main`. `.nojekyll` preserves the static
HTML files. The custom `404.html` carries every module's `go-import` tag; Go picks the one
whose prefix matches, so package paths beneath each module resolve.

After DNS and HTTPS are active, verify both endpoints:

```sh
curl -fsSL 'https://plantcontrol.org/v1/gonum?go-get=1'
curl -sSL 'https://plantcontrol.org/v1/gonum/blas?go-get=1'
```

The module itself will not resolve until `PlantControl/gonum` exists and its
`go.mod` declares `module plantcontrol.org/v1/gonum`.

## Private modules

- Path: `plantcontrol.org/private/<repo>` in a private `PlantControl/<repo>`.
- Developers: `go env -w GOPRIVATE=plantcontrol.org/private` and Git access to
  GitHub (SSH `insteadOf` or `gh auth setup-git`).
- CI: the PlantControl GitHub App mints a read-only token per job. Needs org
  variable `PLANTCONTROL_GO_APP_ID` and org secret `PLANTCONTROL_GO_APP_KEY`;
  install the app on every private module repo. Workflow steps:

```yaml
env:
  GOPRIVATE: plantcontrol.org/private
steps:
  - uses: actions/create-github-app-token@v2
    id: app-token
    with:
      app-id: ${{ vars.PLANTCONTROL_GO_APP_ID }}
      private-key: ${{ secrets.PLANTCONTROL_GO_APP_KEY }}
      owner: PlantControl
      permission-contents: read
  - run: git config --global url."https://x-access-token:${{ steps.app-token.outputs.token }}@github.com/PlantControl/".insteadOf "https://github.com/PlantControl/"
```

- Docker: pass credentials as a BuildKit secret (`--mount=type=secret,id=netrc`),
  never a build argument.
