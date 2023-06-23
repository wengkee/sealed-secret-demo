# Install SealedSecret Controller 
After logging in to OpenShift, the easiest way is to use Helm command to install SealedSecret Controller in OpenShift cluster. By default, it will be installed into `kube-system` namaspace.

    helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

    helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets

# Install Kubeseal
We will need to install a client to seal / encrypt the secret, which is `kubeseal`. As I am using MacOS, it can be easily done using `brew`

    brew install kubeseal

# Sealing the secret (locally)
Here, we will create a secret file, and then seal it with `kubeseal`. We should login to OpenShift cluster before running kubeseal.

    echo -n bar | oc create secret generic mysecret --dry-run=client --from-file=foo=/dev/stdin -o yaml > mysecret.yaml

    kubeseal < mysecret.yaml > mysealedsecret.yaml

# Apply the SealedSecret 
Once we have the SealedSecret file, we should be able to apply this into the OpenShift cluster. The Controller will in turn decrypt it and create a Secret based on it.

    oc create -f mysealedsecret.yaml
    oc get secret mysecret

