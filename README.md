# PlantControl domain

This GitHub Pages site serves Go import metadata for `plantcontrol.org/v1/<repo>`
(`gonum`, `controlsys`, `systemid`, `process-lab`), each pointing at
`https://github.com/PlantControl/<repo>`. To add a module, create
`v1/<repo>/index.html` and add its `go-import` tag to `404.html`.

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
