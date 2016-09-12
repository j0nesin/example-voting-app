.PHONY: build push deploy deploy-delete monitor proxy dashboard

ECR_REPO ?= <insert>
CLUSTER ?= <insert>
REGISTRY ?= $(AWS_ACCOUNT).dkr.ecr.us-east-1.amazonaws.com/$(ECR_REPO)
TAG ?= latest
PROXY_PORT ?= 8011

build:
	cd ../../vote && docker build -t $(REGISTRY)/examplevotingapp_vote:$(TAG) .
	cd ../../worker && docker build -t $(REGISTRY)/examplevotingapp_worker:$(TAG) .
	cd ../../result && docker build -t $(REGISTRY)/examplevotingapp_result:$(TAG) .

push:
	# login to ecr each time due to expiring credentials
	eval $$(aws ecr get-login)
	docker push $(REGISTRY)/examplevotingapp_vote:$(TAG)
	docker push $(REGISTRY)/examplevotingapp_worker:$(TAG)
	docker push $(REGISTRY)/examplevotingapp_result:$(TAG)

deploy-login:
	kubectl config use-context ${CLUSTER}

deploy:
	./ecr-secret.sh
	sed 's|<REGISTRY>|$(REGISTRY)|g;s|<IMAGE_TAG>|$(TAG)|g' deployment.yaml | kubectl apply -f -

deploy-delete:
	sed 's|<REGISTRY>|$(REGISTRY)|g;s|<IMAGE_TAG>|$(TAG)|g' deployment.yaml | kubectl delete -f -

proxy:
	kubectl proxy --port $(PROXY_PORT)
	@echo "Proxy created at http://localhost:$(PROXY_PORT)"

dashboard:
	open http://localhost:$(PROXY_PORT)/api/v1/proxy/namespaces/kube-system/services/kubernetes-dashboard
	
logging:
	open http://localhost:$(PROXY_PORT)/api/v1/proxy/namespaces/kube-system/services/elasticsearch-logging                                                                                            
	open http://localhost:$(PROXY_PORT)/api/v1/proxy/namespaces/kube-system/services/kibana-logging
