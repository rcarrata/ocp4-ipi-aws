# Terraform parameters
ENVIRONMENT       := lab
TERRAFORM         := terraform
# TF_FILES_PATH     := src
TF_BACKEND_CONF   := configuration/backend
TF_VARIABLES      := configuration/tfvars
OCP_VERSION       := 4.7.6


all: init changes deploy

init: 
	$(info Initializing Terraform...)
	$(TERRAFORM) init \
		-backend-config="$(TF_BACKEND_CONF)/$(ENVIRONMENT).conf" $(TF_FILES_PATH)

changes: 
	$(info Get changes in infrastructure resources...)
	$(TERRAFORM) plan \
		-var="aws_access_key_id=$(aws_access_key_id)" \
		-var="aws_secret_access_key=$(aws_secret_access_key)" \
		-var-file="$(TF_VARIABLES)/terraform.tfvars" \
		-out "output/tf.$(ENVIRONMENT).plan" \

deploy: changes
	$(info Deploying infrastructure...)
	$(TERRAFORM) apply \
		output/tf.$(ENVIRONMENT).plan

destroy:
	$(info Destroying infrastructure...)
	$(TERRAFORM) destroy \
		-var="aws_access_key_id=$(aws_access_key_id)" \
		-var="aws_secret_access_key=$(aws_secret_access_key)" \
		-auto-approve \
		-var-file="$(TF_VARIABLES)/terraform.tfvars"
	$(RM) -r .terraform
	$(RM) -r output/tf.$(ENVIRONMENT).plan
	$(RM) -r state/terraform*
	$(RM) -r .terraform.lock.hcl

clean:
	$(info Cleaning unused files...)
	$(RM) -r .terraform
	$(RM) -r output/tf.$(ENVIRONMENT).plan
	$(RM) -r state/terraform*
	$(RM) -r .terraform.lock.hcl