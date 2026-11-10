# OpenPolyfill signing CA

Create the CA once, from the root of this repository. Keep its private key
**outside** the manifest repository:

```sh
umask 077
mkdir -p ../openpolyfill-ca-private certificates
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:3072 \
  -out ../openpolyfill-ca-private/openpolyfill-ca-private.pem
openssl req -new -x509 -sha256 -days 3650 \
  -key ../openpolyfill-ca-private/openpolyfill-ca-private.pem \
  -out certificates/openpolyfill-ca.pem \
  -subj '/CN=OpenPolyfill XPI Signing CA/O=OpenPolyfill' \
  -addext 'basicConstraints=critical,CA:TRUE' \
  -addext 'keyUsage=critical,keyCertSign,cRLSign'
openssl x509 -in certificates/openpolyfill-ca.pem -noout \
  -subject -issuer -fingerprint -sha256
```

Commit and publish only `certificates/openpolyfill-ca.pem`. The public PEM is
the certificate that users explicitly trust through Experiments Authority.

For GitHub Actions signing in each source repository, expose the private PEM
through an Actions secret available to those repositories. For example, an
organization secret limited to WebHID and WebKeyboard:

```sh
gh secret set XPI_CA_PRIVATE_KEY_PEM \
  --org OpenPolyfill \
  --repos WebHID,WebKeyboard \
  < ../openpolyfill-ca-private/openpolyfill-ca-private.pem
```

The private key never belongs in this repository or its Actions configuration.
