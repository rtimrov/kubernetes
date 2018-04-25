brew install kubernetes-helm

Zookeeper:

kubectl create -f zookeeper.yaml

Kafka:

kubectl create -f kafka.yaml

helm init
Prometheus:

git clone https://github.com/kubernetes/charts
cd charts

kubectl create clusterrolebinding tiller-cluster-admin \
    --clusterrole=cluster-admin \
    --serviceaccount=kube-system:default
    
helm install -f prometheus/values.yaml stable/prometheus --name prometheus



Grafana:
    
helm install --name kube-grafana stable/grafana

    
Testing:

kubectl exec -it kafka-0 -- bash

    kafka-topics.sh --create \
    --topic test \
    --zookeeper zk-0.zk-hs.default.svc.cluster.local:2181,zk-1.zk-hs.default.svc.cluster.local:2181,zk-2.zk-hs.default.svc.cluster.local:2181 \
    --partitions 3 \
    --replication-factor 3

    kafka-console-consumer.sh --topic test --bootstrap-server localhost:9092

On differnet terminal:

kubectl exec -it kafka-1 -- bash
    kafka-console-producer.sh --topic test --broker-list localhost:9092
    
