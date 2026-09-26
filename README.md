# PlantControl domain

This GitHub Pages site serves the Go import metadata for
`plantcontrol.org/v1/gonum`. Its repository target is
`https://github.com/PlantControl/gonum`.

The custom domain is `plantcontrol.org`. At Spaceship, configure four apex
`A` records (`@`) for GitHub Pages:

- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

The site is served from the root of `main`. `.nojekyll` preserves the static
HTML files. The custom `404.html` lets Go discover the same repository for
package paths beneath `plantcontrol.org/v1/gonum`.

After DNS and HTTPS are active, verify both endpoints:

```sh
curl -fsSL 'https://plantcontrol.org/v1/gonum?go-get=1'
curl -sSL 'https://plantcontrol.org/v1/gonum/blas?go-get=1'
```

The module itself will not resolve until `PlantControl/gonum` exists and its
`go.mod` declares `module plantcontrol.org/v1/gonum`.
