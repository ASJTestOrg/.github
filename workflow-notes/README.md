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

The activation should be places roughly the same spot as on github

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

To get it to work you might have to allow the repository with the reusable workflow to be accessible from other repositories in your org:
go to repository > settings > actions > general > activate *"Accessible from repositories in the '<orgname>' organization "* > save
This should make the repo accesible for the other repositories inside the organization.

Also, you have to define a trigger for the Caller-workflow (push/pull/dispatch/...)


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


### Triggers and releases
#### On
The on keyword is used to define triggers. These can be `workflow_disatch`, `workflow_call`, push/pull/merge and more.

#### Releases
You can make release by clicking release and drafting a new one. After that you can save the draft, make pre-release og publish.
Release creates a zip of the source.

By using this you can trigger an event that runs a pipeline that has the trigger
```
on:
  release:
    types: [published] <-- This can also be other things like created, saved and so on
```

[More info here on the event](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#release)
[More info on managing releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)


### Secrets and Inputs
You can create action secrets by going into the setting of the repo or organization and then clicking *Actions secrets and variables*

## More info on syntax
[Syntax for github actions](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)