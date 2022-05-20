# k8slab

문서 참고해서 VM (Linux Ubuntu) 생성해서 kubdadm 으로 kebernetes 클러스터 설치하면 된다.
단일노드에 master/work node 같이 설치하기 위해서는 taint 설정을 해 줘야 한다.
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl taint nodes --all node-role.kubernetes.io/control-plane-

강좌에는 master-만 나와 있으나, 실제로는 안된다. control-plnate 도 해줘야 함
(정확히는 taint 설정을 보고, scheduling 관련 문제가 있는 부분을 처리하면 된다)
kubectl get nodes -o json | jq '.items[].spec.taints'  -> 이 명령어로 taint 관련 문제 확인

디스크 size, CPU, Mem 문제로 위의 명령어로 확인 가능

* Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
오류가 발생할 대 아래 명령 실행

unset KUBECONFIG
export KUBECONFIG=/etc/kubernetes/admin.conf
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

한개의 노드에 Worker, Master모두 설치할 때 문서대로 하면 kubeinit 할때 오류가 발생하며 아래의 명령어 실행 필요
(기본적으로 구성되는 config.toml 파일이 이상한 듯)
mkdir -p /etc/containerd && containerd config default > /etc/containerd/config.toml

