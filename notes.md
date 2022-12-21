# Notes

- runc is the actual tool that creates the container by interacting with kernel
  - kata containers use kata-runtime (name: kata, handler: kata)
  - gVisor uses runsc (name: gvisor, handler: runsc)
  - Docker by default use runc but we can use --runtime runsc in docker cli
- Before careful with volume mounts. Mount only with container scope not at pod scope (particularly confusing in deployments)
- pod creation 
  ``` kubectl --dry-run=client -o yaml run podname --image=nginx --command -- sh -c 'sleep infinity'```
- Set auto-completion
  ```
  source <(kubectl completion bash)
  echo "source <(kubectl completion bash)" >> ~/.bashrc
  ```
- delete pod without delay $ kubectl delete pod <podname> --now or $ kubectl delete pod <pod-name> --force --grace-period 0
- quick look up on documentation $ kubectl explain pod --recursive
- kube-bench
  - kube-bench run --targets master --check 1.2.20
- Container security
  - rm bash and sh from /usr/bin
  - set user using USER
  - To share the same PID kernel namespace $ docker run -d --name app2 **--pid=container:app1** -d nginx:alpine sleep infinity
  - tags can be overwritten where as image digest cannot be overwritten
- get exe path from /proc/<pid>/exe
- log files
  -  /var/log/pods
  -  /var/log/containers
  -  journalctl -fu <service>
  - /var/log/syslog
  - crictl logs
- Existing labels won't be allowed to modified once NodeRestriction is enabled using --enable-admission-plugins=NodeRestriction
- **We need to use the annotation on Pod level, not Deployment level**
- Audit logs
  - create the log directory
  - audit log file path and audit policy switches should be added
- Providing access to a given user
  - create key. $ openssl genrsa -out <user>.key 2048
  - create CSR. $ openssl req -new -key <user>.key -out <user>.csr
  - create CRT by signing the CSR $ openssl x509 -req -in <user>.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out 60099.crt -days 500
  - Create a new context for kubectl named 60099@internal.users which uses this CRT to connect to K8s
  ```
  k config set-credentials <user> --client-key=<user>.key --client-certificate=<user>.crt
  k config set-context <context> --cluster=kubernetes --user=<user>
  k config get-contexts
  k config use-context <context>
  ```
- create users steps
  - create a key and a csr using openssl
  - convert csr to base64
  - create a certificate signing request in yaml and set the encoded value
  - get your csr created and approved
  - get the certificate from the approved csr
  - create users using set-credentials using client key and certificate
  - a context will have a mapping between users and a cluster. create a context using set-context
  - activate the context using use-context
- strace can be used to trace syscalls
  - strace <command>
  - strace -p <pid> -f -cw
- secret key will be append as the path to the mounted path
  - mounted path /etc/givenpath and secret is password=dummy; secret is available at /etc/givenpath/password
- Running falco for given rule file
  ```
  falco -M 45 -r monitor_rules.yml > falco_output.log
  ```
- check if gvisor is running $ kubectl exec -n <podname> -- dmesg
- kubectl auth can-i if used for service account should use "--as system:serviceaccount:namespace:sa" and also remember to use the correct namespace
- To scan an image ```$ trivy image --severity=CRITICAL,HIGH nginx | grep CRITICAL```
- For image policy webhook, mount the entire folder path without trailing "/" after the folder name instead of mounting each files
- If shasum or shas512sum is not available then install using ```apt install hashalot```
- Use "echo -n <string"| base64" as "-n" will ignore newline character
- if kubernetes-service-node-port is modified, please delete the associated service and its gets recreated automatically (static pods)
