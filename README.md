# Demo for the configuration of a custom Julia buildpack in Tanzu Application Platform (TAP)
## Package and push buildpack for Tanzu Application Platform
```
export CONTAINER_REGISTRY=<my-registry>
git clone https://github.com/connectedmissionlab/julia-buildpack buildpack
pack buildpack package julia-buildpack --config package.toml
docker tag julia-buildpack:$CONTAINER_REGISTRY/tap-images
docker push $CONTAINER_REGISTRY/tap-images
export BUILDPACK_CONTAINER_TAG=$(docker inspect --format='{{index .RepoDigests 0}}' $CONTAINER_REGISTRY/tap-images)
```

## Configure custom buildpack and builder for TAP
```
envsubst < kpack/buildpack.yaml | kubectl apply -f -
envsubst < kpack/builder.yaml | kubectl apply -f -
# Check status
kubectl get clusterbuilder julia-builder
```

## Apply workload 
```
tanzu apps workload create -f workload.yaml # or kubectl apply -f workload.yaml
```
