## Open VPN
Kubernetes OpenVPN deployment

### Run

Assumes existing configuration present at the NFS path specified.

```bash
helm upgrade --install \
openvpn-ha \
--debug \
--set service.ip=10.0.0.100 \
--set affinity.label=network \
--set affinity.value={fast} \
--set replicaCount=1 \
--set volumes.data.server=10.0.0.10 \
--set volumes.data.path=/openvpn/data \
.
```
