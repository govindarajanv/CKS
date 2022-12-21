# Practice for fluency

## Network Policy
- playaround with networkpolicy based on conditions

## ImagePolicy Webhook



## Falco
- practice changing falco_rules.local.yaml
- redirect the log file to a different log file

## Kube-bench & CIS
- run kube-bench for 
  - ```kube-bench run --targets=master --check 1.2.3```
  - ```kube-bench run --targets=node --check 1.2.3```
## Misc
- Get the name of all the contexts ```kubectl config get-contexts -o name```
- view certificate of all the contexts ```kubectl config view --raw```
- crictl commands 
  - ```crictl pods --name <deployment name>```
  - ```crictl ps --pod <pod name>```

## Platform Binaries
- Install hashalot
- ```shasum -a 512``` or ```sha512sum ```

## OPA
- look for configmaps in opa namespace and get the allowed registries
- edit constraintTemplate in CRDs
- To test an image ```k run opa-test --image=```

## AppArmor
- Remember to add anotations

## Runtimeclass
- Add runtime class in pod manifest after creating the runtime class

## ETCD

```ETCDCTL_API=3 etcdctl \
--cert /etc/kubernetes/pki/apiserver-etcd-client.crt \
--key /etc/kubernetes/pki/apiserver-etcd-client.key \
--cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/<namespace>/<secret>```
