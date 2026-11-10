# Adding a new XPI

1. Create or choose the add-on's source repository. Its XPI must declare a
   stable `browser_specific_settings.gecko.id`.
2. Configure the add-on's own release workflow to sign with
   [`OpenPolyfill/action-xpi-sign`](https://github.com/OpenPolyfill/action-xpi-sign).
   Supply the signing key through that source repository's GitHub Actions
   secrets or an organization secret. Do not place private keys here.
3. Add the source repository to `repositories.yml`, with a unique key, its URL,
   and default branch.
4. Copy `manifests/template-example.yml.template` to `manifests/{name}.yml`.
   Set `repo-prefix` to the repository key, `addon-id` to the exact Gecko ID,
   and `ca-certificate` to the relevant public PEM in `certificates/`.
5. Add the new manifest path to `manifest.json` and commit the metadata.

This repository does not trigger or perform a build or signing operation.
`signing: source-repository` is metadata indicating where signing takes place.
