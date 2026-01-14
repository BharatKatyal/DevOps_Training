kustomize build k8s/

Kustomize allows you to customize raw, template-free YAML files for multiple purposes. It targets kustomization of YAML for Kubernetes.
It will only build the yaml files and not apply them to the cluster.


<!--  If you add the | it pipe the output of kustomize build to kubectl apply the yaml files to the cluster. -->
kustomize build k8s/ | kubectl apply -f -

<!-- If you add the | it pipe the output of kustomize build to kubectl delete the yaml files from the cluster. -->
kustomize build k8s/ | kubectl delete -f -