# Evidence standard

This repository is intended to be a credible engineering portfolio. Evidence must be reproducible, clearly attributed to a test, and safe to publish.

## Screenshot checklist

Before committing a screenshot:

- Crop to the relevant Packet Tracer window or CLI output.
- Ensure the device name, command, and meaningful result are visible.
- Hide passwords, enable secrets, tokens, private keys, personal data, and unrelated desktop content.
- Use descriptive lowercase filenames, such as `dns-nslookup-success.png`.
- Add a short Markdown caption stating what the screenshot demonstrates and what it does not prove.

## Configuration evidence

- Prefer targeted `show` commands over full running-configuration dumps.
- Remove authentication and cryptographic material before publishing any configuration excerpt.
- Include test conditions (source VLAN / host, destination, protocol, and direction) where relevant.
- Distinguish configured state from verified behavior. A configured ACL does not by itself prove that the desired traffic is allowed or denied.

## Competition materials

Do not upload official starter topologies, assessment guides, PDFs, or other third-party material unless redistribution permission is explicit. Link to the official source instead.
