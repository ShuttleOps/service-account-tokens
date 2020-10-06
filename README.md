# service-account-tokens

In this repo you will find Kubernetes resources for creating a ShuttleOps service account.

The `shuttleops-role-sa-binding.yaml` file can be used to create a ShuttleOps service account, Cluster Role and Role Binding which will allow ShuttleOps to deploy and manage your application.

The current supported versions of kubernetes are:

- 1.16
- 1.17
- 1.18

## Gathering the required information

To connect a Kubernetes cluster to ShuttleOps, we will need the CACert, API URL, token and Kubernetes version.

### How to view your CACert and API URL

**Create a text file with your cluster config**:

```
kubectl config view --flatten --minify > cluster-config.txt
```

Next, open the file and copy the following values:

- `certificate-authority-data` - This is your CACert
- `server` - This is your API URL

### How to generate a token

Before you generate a token, create a Shuttleops Service Account and a Cluster Role, then finally bind the Cluster Role to the Service Account. You can easily do that by applying the YAML file we've included in this repository.

**Create the required Kubernetes resources**:

```
kubectl apply --file https://raw.githubusercontent.com/ShuttleOps/service-account-tokens/0.1.0/shuttleops-role-sa-binding.yaml
```

**Fetch the token name**:

```
TOKENNAME=`kubectl --namespace kube-system get serviceaccount/shuttleops --output jsonpath='{.secrets[0].name}'`
```

**View the token (Mac and Linux)**:

```
kubectl --namespace kube-system get secret $TOKENNAME --output jsonpath='{.data.token}'| base64 --decode
```

**View the token (Windows)**:

```
TOKEN_BASE64=`kubectl --namespace kube-system get secret $TOKENNAME --output jsonpath='{.data.token}'`
[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String(${TOKEN_BASE64}))
```

