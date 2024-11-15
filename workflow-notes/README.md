# Workflow and github actions notes:

## Workflow templates 
### github:
Templates works by creating an organization and making a public *(special)* repository called .github
In this repository you can make a template by creating a folder called 'workflow-templates' that contains the workflow.yml and a properties.json file *(the two files must have the same name)*.
The yml is the workflow template and the properties.json is the metadata-description for said template.

To use this in another repo, you must go into the repo and click actions > configure on the template of choice.

On github you can only use custom templates on public repos unless you have github one subscription *(or whatever it's called)*

### Gitea:
**DO Later**


## Workflow Dispatch
### Github
You can add a dispatch to your workflow by writing `workflow_dispatch:`
**Add additonal information**

To activate a workflow manually in github. you go into actions > choose the workflow on the left-hand side > and then it should say **Run Workflow** on the right-hand side in a blueish box

### Gitea
Gitea does not currently have this feature, but it is planned for release in 1.23, though the exact release time is unknown.

## Reusable workflow
### Github

### Gitea

## Artifacts
### Github

### Gitea


## General Workflow stuff
### Keywords
#### On

### Tags

### Releases

### Triggers

### Actions/Workflows/Jobs
#### Actions
**Unsure if this is available in gitea**
#### Workflow
#### Jobs

### Secrets and Inputs