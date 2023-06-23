# Install SealedSecret Controller 
    helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

    helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets

# Install Kubeseal on MacBook
    brew install kubeseal

# Create a Secret and seal it
    echo -n bar | oc create secret generic mysecret --dry-run=client --from-file=foo=/dev/stdin -o yaml > mysecret.yaml

    kubeseal < mysecret.yaml > mysealedsecret.yaml

# Apply the SealedSecret and see it works!
    oc create -f mysealedsecret.yaml
    oc get secret mysecret

