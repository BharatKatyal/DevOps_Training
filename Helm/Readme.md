

helm search hub wordpress

helm repo add bitnami https://charts.bitnami.com/bitnami


helm search repo wordpress
```
NAME                    CHART VERSION   APP VERSION     DESCRIPTION                                       
bitnami/wordpress       28.1.2          6.9.0           WordPress is the world's most popular blogging ...
bitnami/wordpress-intel 2.1.31          6.1.1           DEPRECATED WordPress for Intel is the most popu...
```

helm repo list
```
NAME    URL                                 
bitnami https://charts.bitnami.com/bitnami  
fluent  https://fluent.github.io/helm-charts
```


Can install the same release on the cluster by running the same command multiple times. The release name has to be unique.
helm install [release-name] [chart-name]
helm install release1 bitnami/wordpress
helm install release2 bitnami/wordpress
helm install release3 bitnami/wordpress




Helm uninstall [release-name]
helm uninstall release1
helm uninstall release2
helm uninstall release3


if you only want to pull the chart to your local machine and not install it on the cluster, you can use the pull command
--untar will untar the chart after pulling it. Normally the chart is pulled in a compressed format.
helm pull [chart-name]
helm pull --untar bitnami/wordpress

ls wordpress
```
Chart.yaml  README.md  templates  values.schema.json  values.yaml
```

You can make changes to it and once you are ready to install it on the cluster, you can use the install command
helm install [release-name] [chart-name]
helm install release1 ./wordpress