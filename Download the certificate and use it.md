Now, as the requesting user, you can download the issued certificate and save it to a server.crt file by running the following:

kubectl get csr my-svc.my-namespace -o jsonpath='{.status.certificate}' \
    | base64 --decode > server.crt
Now you can populate server.crt and server-key.pem in a Secret that you could later mount into a Pod (for example, to use with a webserver that serves HTTPS).

kubectl create secret tls server --cert server.crt --key server-key.pem
secret/server created
Finally, you can populate ca.pem into a ConfigMap and use it as the trust root to verify the serving certificate:

kubectl create configmap example-serving-ca --from-file ca.crt=ca.pem
configmap/example-serving-ca created
