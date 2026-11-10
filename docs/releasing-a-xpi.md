# Releasing an XPI

Each add-on repository owns its own build, signing and GitHub Release.

1. Build the unsigned XPI in the source repository.
2. Sign it there using the publisher's CA and
   [`OpenPolyfill/action-xpi-sign`](https://github.com/OpenPolyfill/action-xpi-sign).
3. Attach the resulting signed XPI to that add-on's GitHub Release. Use a
   distinct asset name for the EA build when the same release also publishes
   an AMO-signed XPI.

Example steps for an add-on's workflow (after building `dist/addon.xpi`):

```yaml
- uses: actions/checkout@v4
  with:
    repository: OpenPolyfill/xpi-manifest
    path: xpi-manifest

- name: Sign XPI
  id: xpi-sign
  uses: OpenPolyfill/action-xpi-sign@v1
  with:
    source: dist/addon.xpi
    artifacts: signed
    privateKey: ${{ secrets.XPI_CA_PRIVATE_KEY_PEM }}
    certificate: xpi-manifest/certificates/openpolyfill-ca.pem
    expectedId: webhid@k4zoku.dev
```

The signed file is available at `steps.xpi-sign.outputs.target` for subsequent
upload/attestation steps in that same workflow. Adapt `source` and `expectedId`
for each add-on.

No release signing happens in the `xpi-manifest` repository.
