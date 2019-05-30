---
description: How to encrypt and use a .env file for a service hosted on S3.
---

# Encrypting service configuration for S3

Apps are configured by putting files in a secure configuration bucket on S3. The `ENTRYPOINT`script for our apps pulls all files in from the app’s path in the bucket before starting up. This allows an app to be securely configured with a `.env` file and, for example, `server.crt` and `server.key` files for TLS connections.

## Encrypting Environment Variables

Though `.env` files are stored encrypted in S3 and only transferred securely, we still encrypt environment variables like passwords so that they are not seen in plain when editing the `.env`files.

Each service is configured to have its own private key in the Amazon KMS keystore. Only the task role may decrypt with that key.

Adding the `_KMS_ENCRYPTED` suffix to an environment variable’s name in the `.env` file will cause the task to decrypt the variable at runtime, storing it in `process.env` after stripping the suffix.

To create an encrypted environment variable value:

1. Visit the “Customer managed keys” section of the KMS part of the AWS web console.
2. Look for the **Config Key Alias** for your service. Save it in the `$CONFIG_KEY_ALIAS` environment variable.
3. Log in to AWS CLI with an account that has `KMS::encrypt` permissions for the key.
4. Run the following command **with a leading space so that it doesn’t appear in your command history**: `aws kms encrypt --output text --key-id $CONFIG_KEY_ALIAS --plaintext …`
5. Copy the encrypted value \(the output up to the whitespace\) as the value in `.env`: `PASSWORD_KMS_ENCRYPTED=…`

{% hint style="info" %}
Note that using `--plaintext` on the command line will cause `aws kms` to encrypt the ASCII as-is. When using the `fileb://` form to reference a file on disk, `aws kms` will first Base64 encode the value, which will cause a failure on the app side, which does not expect Base64-encoded values.
{% endhint %}
