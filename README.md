#### mediawiki ####


### A brief about the repo ###

- The repo has 3 folders. "mediawiki_app" has all the configs for the app itself. "mysql_db" has all configs related to DB. "terraform" has tf code along with pipeline to deploy it.

- Per the ask, I've chosen "Kustomize" as my go to tool to manage my manifests and I've used "ArgoCD" to deploy the app. All the manifests adhere to the standard structure of kustomize and have a base and overlap manifests.

- Per the ask, I've written IaC using terraform and deployed Infrastructure on Azure.

### About the app ###

- To deploy the app,
    1) A CI pipeline can be built for the CI stage that does a few basic steps. I would do it in Azure DevOps and make use of AzDo tasks. Just to illustrate the flow, giving an ex:
        docker build -t app_name:$(Build.BuildNumber)
        docker push acr_name

    2) Setup project and app on ArgoCD for continuous deployment on the correct branch. As soon as the tag is updated. the ArgoCD hook kicks in and deploys the app.

### About the Infra ###

- To deploy tf,
    1) All variables consumed by the pipeline can be passed by AzDo library. Create the following variables in library:
       subscription_id: #Subscription ID
       tf_rg_name: #Resource Group name that houses storage account to store state file
       tf_rg_location: #Resource Group location that houses storage account to store state file
       tf_sa_name: #Storage account name to store state file
       tf_sa_sku: #SKU of SA 
       tf_sa_container_name: #Blob name to store state file
       client_id: #Client ID of SPN having access to create resources in the Subscription
       client_secret: #Secret of the SPN
       tenant_id: #Tenant ID of the subscription

    2) Update the tfvars file to match the requirements. I've left the values I used as is to give a better idea.

    3) Speaking of the tf code itself, it's modularised and VNET and Subnet modules are written in a way that's more inclined towards platform engineering teams (that requires high level of resusability) and k8's module, a very simple snippet aligned towards app teams.



### Screenshots ###


1)Mediawiki deployed successfully in ArgoCD: screenshots/mediawiki_deployed_in_argocd.png

2)Installation of mediawiki successful with both app and db working as expected: screenshots/successful_setup_of_mediawiki.png






