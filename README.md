# OpenShift APIs for Data Protection (OADP) Demo

This repo contains supporting files for OADP demo for this blog entry: [Back up Kubernetes persistent volumes using OADP](https://developers.redhat.com/articles/2023/08/07/back-kubernetes-persistent-volumes-using-oadp)

## Deploy sample application: WordPress
We need to deploy a Kubernetes application that uses persistent volumes. We will use WordPress, which uses the MySQL database. The instructions were based on this link.

1. Download the MySQL deployment configuration file:
    ```
    curl -LO \
    https://raw.githubusercontent.com/yortch/oadp/main/mysql-deployment.yaml
    ```
1. Download the WordPress configuration file:
    ```
    curl -LO \
    https://raw.githubusercontent.com/yortch/oadp/main/wordpress-deployment.yaml
    ```
1. Export PASSWORD as an environment variable:
    ```
    PASSWORD=<YOUR_PASSWORD>
    ```
1. Generate the following kustomization.yaml file:
    ```
    cat <<EOF >./kustomization.yaml
    secretGenerator:
    - name: mysql-pass
    literals:
    - password=${PASSWORD}
    resources:
    - mysql-deployment.yaml
    - wordpress-deployment.yaml
    EOF
    ```
1. Create a new application project:
    ```
    oc new-project wordpress
    ```
1. Apply the kustomization.yaml file:
    ```
    oc apply -n wordpress -k ./
    ```
1. Next, expose the WordPress service:
    ```
    oc expose service wordpress -n wordpress
    ```
1. Print the service URL and navigate to it from a browser.
    ```
    echo http://$(oc get route -n wordpress -o jsonpath='{.items[0].spec.host}')
    ```
1. Proceed with the initial WordPress setup so that it is included in the backup.
