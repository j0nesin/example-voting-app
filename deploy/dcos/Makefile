.PHONY: settings build push deploy deploy-delete monitor proxy dashboard

TAG ?= latest

settings:
	@echo "\nCLUSTER = ${CLUSTER}\nREGISTRY = ${REGISTRY}\n"

deploy-login:
	dcos config set core.dcos_url $(CLUSTER)
	dcos auth login

build:
	docker build -t $(REGISTRY)/examplevotingapp_vote:$(TAG) ../../vote
	docker build -t $(REGISTRY)/examplevotingapp_worker:$(TAG) ../../worker
	docker build -t $(REGISTRY)/examplevotingapp_result:$(TAG) ../../result

push:
	docker push $(REGISTRY)/examplevotingapp_vote:$(TAG)
	docker push $(REGISTRY)/examplevotingapp_worker:$(TAG)
	docker push $(REGISTRY)/examplevotingapp_result:$(TAG)

deploy:

deploy-delete:

dashboard:
	open $(CLUSTER)
	open $(CLUSTER)/marathon
	
logging:

ssh-node:
	dcos node ssh --master-proxy --leader