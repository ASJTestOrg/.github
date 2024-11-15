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
Reusable workflow are workflow made to be reused by other repositories workflow, making it easier to create workflows and keeping duplicate code down.
Reusable workflows are typically called *Called* and workflows using reusable worksflows are called *Callers*.

They, of course, need to recide in the folder .github > workflows

You can max nest 4 workflows down. 
example: Caller > workflow A > workflow B > workflow C 
This example is 3 nested workflows

You can atmost call 20 reusable workflows in one workflow file. Nested counts.

Secrets can be shared between workflows, but env context cannot be shared either. env context are local to the workflow


To Use a reusable workflow you must first define a workflow to be reusable by adding `on: workflow_call:` to it. Thereafter you can add other stuff, like what input or secret it gets from the caller.
```
on:
  workflow_call:
    inputs:
      config-path:
        required: true
        type: string
    secrets:
      personal_access_token:
        required: true
```

Here the name of the input is called *config-path* and the secret *personal_access_token*

Over in the Caller workflow you can use this reusable workflow by doing so:
```
jobs:
  reusable_workflow_job:
    runs-on: ubuntu-latest
    steps:
    - uses: <insert workflow path here> (typically stored in another repo, so you need to add org/repo/name-of-workflow.yml@branch(if needed))
      with:
        repo-token: ${{ secrets.personal_access_token }}
        configuration-path: ${{ inputs.config-path }}
```
For the secret you can also use the `inherit` keyword instead of `${{ secrets.personal_access_token }}` to inherit the secrets to the called-workflow. 
This can only be done if both workflows recide in the same organization and shares secret.

You can also use a matrix with reusable workflows, so they run x amount of times with x,y,z values.
```
jobs:
  ReuseableMatrixJobForDeployment:
    strategy:
      matrix:
        target: [dev, stage, prod]
    uses: octocat/octo-repo/.github/workflows/deployment.yml@main
    with:
      target: ${{ matrix.target }}
```

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