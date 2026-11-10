# XPI manifest repo

Manifest repository for OpenPolyfill Firefox extensions distributed with
publisher-managed XPI signatures.

This repository contains **public CA certificates, source-repository metadata,
and per-add-on manifests**. It does not build, sign, or publish XPIs. Each add-on
repository owns its release pipeline and signs using
[`OpenPolyfill/action-xpi-sign`](https://github.com/OpenPolyfill/action-xpi-sign).
Signed artifacts are distributed through the add-on's own GitHub Releases.

- [Adding a new XPI](docs/adding-a-new-xpi.md)
- [Releasing an XPI](docs/releasing-a-xpi.md)
- [Creating and publishing the CA](docs/signing-ca.md)

## Layout

| Path | Purpose |
| --- | --- |
| `manifest.json` | Public index of this publisher and manifest entries |
| `repositories.yml` | Registered source repositories |
| `manifests/*.yml` | Individual XPI metadata, inspired by Mozilla's xpi-manifest |
| `certificates/*.pem` | Public signing CA certificate(s) |
| `docs/` | Publisher and release procedure |

The metadata layout is inspired by
[`mozilla-extensions/xpi-manifest`](https://github.com/mozilla-extensions/xpi-manifest),
but is not a Mozilla Taskcluster manifest or a supported EA URL-import schema.
Experiments Authority v1.0.0 currently accepts a PEM certificate directly via
**about:addons → Experiments → Trusted certificate authorities → Add CA**.

## Creating the GitHub repository

After adding the public CA certificate, from this directory:

```sh
git init -b main
git add .
git commit -m 'Initial OpenPolyfill XPI manifests'
gh repo create OpenPolyfill/xpi-manifest --public --source=. --remote=origin --push
```

## License

MPL-2.0.
