# Locust.io を k8s 上で動かしてみる

サンプルコード群は[GitHub - GoogleCloudPlatform/distributed-load-testing-using-kubernetes: Distributed load testing using Kubernetes on Google Container Engine](https://github.com/GoogleCloudPlatform/distributed-load-testing-using-kubernetes)からお借りしました。

ローカルクラスタは kind で作成する。
kind でローカルレジストリを利用できるようにする。
参考: [kind - Local Registry](https://kind.sigs.k8s.io/docs/user/local-registry/)

```
./scripts/create-kind-with-registry.sh
```

負荷試験対象のサンプルアプリをデプロイ

```
$ cd sample-app
$ docker build -t localhost:5000/smaple-app:latest .
$ docker push localhost:5000/smaple-app:latest
$ cd k8s
$ kubectl apply -f app.yaml
```

Locust 関連のリソースをデプロイ

```
$ cd locust
$ docker build -t localhost:5000/locust-sample:0.1.0 .
$ docker push localhost:5000/locust-sample:0.1.0
$ cd k8s apply -f locust-worker-controller.yaml
$ cd k8s apply -f locust-master-controller.yaml
$ cd k8s apply -f locust-master-service.yaml
```

## Port Fowarding

```
kubectl port-forward svc/locust-master 8089:8089
```

ブラウザで `http://localhost:8089` にアクセスすると GUI 上で負荷試験を実行できる。
