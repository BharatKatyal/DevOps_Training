kustomize build k8s/

Kustomize allows you to customize raw, template-free YAML files for multiple purposes. It targets kustomization of YAML for Kubernetes.
It will only build the yaml files and not apply them to the cluster.


<!--  If you add the | it pipe the output of kustomize build to kubectl apply the yaml files to the cluster. -->
kustomize build k8s/ | kubectl apply -f -

<!-- If you add the | it pipe the output of kustomize build to kubectl delete the yaml files from the cluster. -->
kustomize build k8s/ | kubectl delete -f -


Kubectl apply -k k8s/

Common Ttansformations
CommonLabels - adds a label to all kubernetes reosurces
namePrefix/Suffix  - adds to beginning or after
Annotiation - like branch



Image Transformer - Change the Image name
Image Transformer can changethe tag too
kustomize.yaml
image:
    - name:ngnix
      newNameL haproxy
      newTag: 2.4
Transformers

 kustomize build k8-organized/
I added  below which applied to everything
commonLabels:
  department: engineering




Kustomize Patches
Patches provide another method to modifying kubernetes configs
unlike common transformers, patches provide a more surgical approch to targeting one or more speciifcation sections in a kubernetes resource
to create a patch 3 paramters muyst be provided
operation type : add/remove/replace

Target : what resource should this patch be applied on?
- kind
-version/group
- name
- namespace
- labelSelector
- AnnotationSelection

Value : what is the avlue that will be either replaced or added with(only needed for add/replac operations)


