# k3dolan

A little [k3d](https://github.com/rancher/k3d) helper's helper.

# features
- creates a single node k3s *and* fetches kubeconfig with one command
- picks a random port for the master preventing port conflicts
- optionally proxies the ingress controller to hosts localhost:80 *when* *needed*

# demo
```
$ k3dolan create test
INFO[0000] Created cluster network with ID b136863fff4f401748de2df59f32327b29b5158e807d265e47de983cef7020b5
INFO[0000] Created docker volume  k3d-test-images
INFO[0000] Creating cluster [test]
INFO[0000] Creating server using docker.io/rancher/k3s:v1.0.1...
INFO[0001] SUCCESS: created cluster [test]
INFO[0001] You can now use the cluster with:

export KUBECONFIG="$(k3d get-kubeconfig --name='test')"
kubectl cluster-info
waiting for kubeconfig...
waiting for kubeconfig...
waiting for kubeconfig...
waiting for kubeconfig...


k3dolan done! now run

    export KUBECONFIG=$(k3dolan kubeconfig test)

$ export KUBECONFIG=$(k3dolan kubeconfig test)

$ kubectl get node
NAME              STATUS   ROLES    AGE   VERSION
k3d-test-server   Ready    master   10s   v1.16.3-k3s.2

$ k3dolan proxy:enable test
proxy to k3d-test-server enabled with --publish 127.0.0.1:80:80 --publish 127.0.0.1:443:443

$ curl localhost
404 page not found

$ k3dolan proxy:disable test
proxy disabled

$ curl localhost
curl: (7) Failed to connect to localhost port 80: Connection refused
```

# install

```
git clone https://github.com/matti/k3dolan
cd k3dolan
ln -s $(pwd)/bin/k3dolan /usr/local/bin
```

# usage
```
USAGE: k3dolan COMMAND [ARGS]
list
  lists all k3s containers

create NAME
  creates a k3s container and fetches the kubeconfig

delete NAME
  deletes a k3s container

kubeconfig NAME
  returns the path to the kubeconfig. use with: export KUBECONFIG=$(k3dolan kubeconfig NAME)

proxy:enable NAME [--publish=127.0.0.1:80:80 --publish=127.0.0.1:443:443]
  publishes ports from the container to the host

proxy:disable NAME
  disables the proxy

proxy:show NAME
  shows the current proxy if enabled

ip NAME
  returns the IP address of the container

stats NAME
  shows docker stats for the container

kubectl|k NAME <kubectl args>
  runs kubectl with correct kubeconfig

helm NAME <helm args>
  runs helm with correct kubeconfig

version|--version
usage|help
```

## related
- https://github.com/matti/k3sup-multipass
- https://github.com/matti/kindol
