## Ressources

https://github.com/kubernetes-sigs/kustomize/tree/master/examples/helloWorld

https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/

## VARS

DEMO_HOME=./
BASE=$DEMO_HOME/base

## Compare overlays


`DEMO_HOME` contains:

 - a _base_ directory - a slightly customized clone
   of the original configuration, and

 - an _overlays_ directory, containing the kustomizations
   and patches required to create distinct _staging_
   and _production_ [variants] in a cluster.

Review the directory structure and differences:

<!-- @listFiles -->
```
tree $DEMO_HOME
```

Expecting something like:

> ```
> /tmp/tmp.IyYQQlHaJP1
> ├── base
> │   ├── configMap.yaml
> │   ├── deployment.yaml
> │   ├── kustomization.yaml
> │   └── service.yaml
> └── overlays
>     ├── production
>     │   ├── deployment.yaml
>     │   └── kustomization.yaml
>     └── staging
>         ├── kustomization.yaml
>         └── map.yaml
> ```

Compare the output directly
to see how _staging_ and _production_ differ:

<!-- @compareOutput -->
```
diff \
  <(kustomize build $OVERLAYS/staging) \
  <(kustomize build $OVERLAYS/production) |\
  more
```

The first part of the difference output should look
something like

> ```diff
> <   altGreeting: Have a pineapple!
> <   enableRisky: "true"
> ---
> >   altGreeting: Good Morning!
> >   enableRisky: "false"
> 8c8
> <     note: Hello, I am staging!
> ---
> >     note: Hello, I am production!
> 11c11
> <     variant: staging
> ---
> >     variant: production
> 13c13
> (...truncated)
> ```


## Deploy

The individual resource sets are:

<!-- @buildStaging @testAgainstLatestRelease -->
```
kustomize build $OVERLAYS/staging
```

<!-- @buildProduction @testAgainstLatestRelease -->
```
kustomize build $OVERLAYS/production
```

To deploy, pipe the above commands to kubectl apply:

> ```
> kustomize build $OVERLAYS/staging |\
>     kubectl apply -f -
> ```

> ```
> kustomize build $OVERLAYS/production |\
>    kubectl apply -f -
> ```