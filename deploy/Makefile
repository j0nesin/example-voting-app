.PHONY: build push deploy-initial deploy

REGISTRY = us.gcr.io/voting-appp
TAG = latest

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
#	kubectl apply -f k8s/deployment.yaml
	sed 's/<IMAGE_TAG>/$(TAG)/g' k8s/deployment.yaml | kubectl apply -f -

deploy-delete:
#	kubectl delete -f k8s/deployment.yaml
	sed 's/<IMAGE_TAG>/$(TAG)/g' k8s/deployment.yaml | kubectl delete -f -
	
monitoring:
	open https://app.google.stackdriver.com/gke
