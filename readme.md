# dask gateway cluster

## Overall goals and benefits
To be able to launch and configure dask cluster workers for parallell processing for ML training and large data loads from within python with minimal effort.

## installation
### server
conda install -c conda-forge dask-gateway-server

kan be done on kubernetes, still working on making that work properly

### client (match version of the server, now lates is 0.7.1)
conda install -c conda-forge dask-gateway



## kubernetes control
uses the kube config file dask-cluster.cfg
make kubernetes find it by:
export KUBECONFIG=dask-cluster.cfg (path this smarter so you can move around in your folders)

(note: helm reads this KUBECONFIG too)
(note2: not pushing this cfg to git for passwords / secrets. ask for it)

## then you can do:
kubectl --namespace dask-gateway get deployment
NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
gateway-dask-gateway           1/1     1            1           57d
scheduler-proxy-dask-gateway   1/1     1            1           57d
web-proxy-dask-gateway         1/1     1            1           57d


kubectl --namespace dask-gateway get pods

kubectl --namespace dask-gateway log gateway-dask-gateway-65f5485c8b-s7bmg



# helm
helm env

helm --namespace dask-gateway list
NAME        	NAMESPACE   	REVISION	UPDATED                               	STATUS  	CHART             	APP VERSION
dask-gateway	dask-gateway	4       	2020-04-17 10:44:43.9856887 +0200 CEST	deployed	dask-gateway-0.7.1	0.7.1

helm uninstall [NAME]

## helm install
(need to have the helm repo for dask-gateway added for this to work)

helm repo list

RELEASE=dask-gateway
NAMESPACE=dask-gateway
VERSION=0.7.1

helm upgrade --install --namespace $NAMESPACE --version $VERSION --values values.yaml $RELEASE dask-gateway/dask-gateway
  Release "dask-gateway" has been upgraded. Happy Helming!


# check and get the ips. need external ip to talk
kubectl get service --namespace dask-gateway
 if its shown as pending, you have a problem.


# what is working and what is not at the moment
the deployment looks ok on the kude cluster but does not get an external ip.
I suspect that it need to be configure in the values.yaml file with the proper external ip and port. Devops team?

When it has an external ip, on the previous version 0.6.1, I had problem launching new workers / gateways? on the kubernetes cluster, but it worked great locally. Maybe the problem is gone now with version 0.7.1? or still needs to be solved.


