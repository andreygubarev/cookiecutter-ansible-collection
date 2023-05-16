.PHONY: help
help: ## Show this help
	@egrep -h '\s##\s' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

ANSIBLE_VERSION := {{ cookiecutter.ansible_version }}
ANSIBLE_VIRTUALENV ?= {{ cookiecutter.ansible_virtualenv}}
ANSIBLE_PYTHON := $(ANSIBLE_VIRTUALENV)/bin/python3
ANSIBLE_PIP := $(ANSIBLE_PYTHON) -m pip
ANSIBLE_LINT := $(ANSIBLE_VIRTUALENV)/bin/ansible-lint
ANSIBLE_GALAXY := $(ANSIBLE_VIRTUALENV)/bin/ansible-galaxy

.PHONY: all
all: $(ANSIBLE_VIRTUALENV) ## Create local environment

.PHONY: clean
clean: ## Remove cache
	rm -rf .cache *.tar.gz

.PHONY: build
build: ## Build collection archive
	$(ANSIBLE_LINT)
	$(ANSIBLE_GALAXY) collection build --force

.PHONY: release
release: build ## Release collection
	open https://galaxy.ansible.com/my-content/namespaces

$(ANSIBLE_VIRTUALENV):
	python3 -m venv $(ANSIBLE_VIRTUALENV)
	$(ANSIBLE_PIP) install -U pip setuptools wheel
	$(ANSIBLE_PIP) install \
		ansible-lint \
		ansible>=$(ANSIBLE_VERSION)
