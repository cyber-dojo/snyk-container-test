
A composite workflow to:
- Harden the github runner
- Load IMAGE_NAME from its registry
- Setup snyk CLI
- Load the .snyk policy file 
- Do a snyk-container-test
- Setup the Kosli CLI
- Kosli attest the snyk sarif file

Requires the following environment-variables:
- IMAGE_NAME 
- KOSLI_FINGERPRINT
- SARIF_FILENAME

Typical use is as follows:

```yml
name: Main

...

jobs:
  ...
  snyk-container-scan:
    needs: [build-image]
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: write
    env:
      IMAGE_NAME:        ${{ needs.build-image.outputs.tagged_image_name }}
      KOSLI_FINGERPRINT: ${{ needs.build-image.outputs.digest }}
      SARIF_FILENAME:    snyk.container.scan.json
    steps:
        uses: cyber-dojo/snyk-container-test@main
        with:
          attestation_name: saver.snyk-container-scan
  ...
```
