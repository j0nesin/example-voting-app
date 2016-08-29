.PHONY: build push deploy deploy-delete monitor proxy dashboard

REGISTRY ?= us.gcr.io/voting-appp
TAG ?= latest
CLUSTER ?= voting-app
PROXY_PORT ?= 8011

build:
	cd ../vote && docker build -t $(REGISTRY)/examplevotingapp_vote:$(TAG) .
	cd ../worker && docker build -t $(REGISTRY)/examplevotingapp_worker:$(TAG) .
	cd ../result && docker build -t $(REGISTRY)/examplevotingapp_result:$(TAG) .

push:
	gcloud docker push $(REGISTRY)/examplevotingapp_vote:$(TAG)
	gcloud docker push $(REGISTRY)/examplevotingapp_worker:$(TAG)
	gcloud docker push $(REGISTRY)/examplevotingapp_result:$(TAG)

deploy-login:
	gcloud container clusters get-credentials $(CLUSTER)

deploy:
	sed 's/<IMAGE_TAG>/$(TAG)/g' k8s/deployment.yaml | kubectl apply -f -

deploy-delete:
	sed 's/<IMAGE_TAG>/$(TAG)/g' k8s/deployment.yaml | kubectl delete -f -

proxy:
	kubectl proxy --port $(PROXY_PORT)
	@echo "Proxy created at http://localhost:$(PROXY_PORT)"

dashboard:
	open http://localhost:$(PROXY_PORT)/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard
	
monitor:
	open https://app.google.stackdriver.com/gke
