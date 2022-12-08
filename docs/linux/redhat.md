# REDHAT Specific

You get an issue like **EE crypto too weak.**

does ths,
```
update-crypto-policies --show
update-crypto-policies --set DEFAULT
```

usually set to FUTURE


### Docker conflicts

When installing docker there are conflicts with podman and containerd
``
yum erase podman buildah
``
