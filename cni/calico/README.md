Steps to convert

1. Remove metalLB
2. Stop k3s
3. Change all external IP annotations in the charts to use the calico way
4. Add `--no-flannel` to k3s
5. Start k3s
6. Deploy Calico
    - install calico operator and CRDs
      ```sh
      kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
      ```
    - kubectl apply -f cni/calico/calico-bgpconfiguration.yaml
    - kubectl apply -f cni/calico/calico-bgppeer.yaml
    - kubectl apply -f cni/calico/calico-installation.yaml


## A possible live migration path

Note: Add my specific calico BGP info somehow first

https://docs.projectcalico.org/getting-started/kubernetes/flannel/migration-from-flannel