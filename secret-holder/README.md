# Helm Secret Chart

This Helm chart creates a Kubernetes `SecretProviderClass` and a Pod that utilizes Key Vault to manage secrets. The secrets are mounted into the Pod, allowing applications to access them securely.

## Prerequisites

- Kubernetes cluster
- Helm 3.x
- Azure Key Vault

## Installation

To install the chart, use the following command:

```bash
helm install <release-name> ./helm-secret-chart
```

Replace `<release-name>` with your desired release name.

## Configuration

The following parameters can be configured in the `values.yaml` file:

- `keyvaultName`: The name of the Azure Key Vault from which to retrieve secrets.
- `secrets`: A list of secrets to retrieve from the Key Vault. Each secret can specify:
  - `name`: The name of the secret in Key Vault.
  - `type`: The type of the secret (e.g., `Opaque`).
  - `version`: (Optional) The version of the secret to retrieve.
- `tenantId`: The Azure tenant ID.

### Example `values.yaml`

```yaml
provider: azure
usePodIdentity: false
useManagedIdentity: false
keyvaultName: my-keyvault
secrets:
  - name: my-secret
    type: Opaque
    version: v1
  - name: another-secret
    type: Opaque
tenantId: my-tenant-id
```

## Generated Resources

Upon installation, the chart will create:

- A `SecretProviderClass` resource configured with the specified parameters.
- A Pod named `secret-holder-<POSTFIX>` where `<POSTFIX>` is a randomly generated 5-character string.
- Secrets named `secret-creator-<POSTFIX>` for each secret defined in the `values.yaml`, mounted under `/dev/null`.

## Notes

After installation, you can check the status of the created resources using:

```bash
kubectl get secretproviderclass
kubectl get pods
```

For further details, refer to the official Helm documentation.