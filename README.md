# REACT_JAVA_PIPELINE_K8S_WITH-HELM
REACT_JAVA_PIPELINE_K8S_WITH HELM 

1) My docker-registry is http nexus. Taking into consideration it's insecure with must configure our /etc/docker/daemon.json with it:
{
  "insecure-registries" : ["registry.example.com:5050"]
}

vim /etc/default/docker
DOCKER_OPTS="--insecure-registry registry.example.com:5050"

systemctl restart docker

2)

we use jq for taking output of package json we need install json query in gitlab-runner

apt install jq -y

3) If we use containerd in K8S we must configure our /etc/containerd/config.toml on all nodes. Master, Workers. Because when we pull image, where the pod is created(Node2) for example 
he sends request from this Node2(Worker) 

vim /etc/containerd/config.toml

version = 2

[plugins]
  [plugins."io.containerd.grpc.v1.cri"]
    [plugins."io.containerd.grpc.v1.cri".containerd]
      default_runtime_name = "runc"
      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
          runtime_type = "io.containerd.runc.v2"
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.test-handler]
          runtime_type = "io.containerd.runc.v2"
    [plugins."io.containerd.grpc.v1.cri".registry]
      [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
        [plugins."io.containerd.grpc.v1.cri".registry.mirrors."172.16.8.22:9001"                                                                                             ]
          endpoint = ["http://172.16.8.22:9001"]
          
4) after that everything is work
