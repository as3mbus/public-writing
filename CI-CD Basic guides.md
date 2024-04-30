---
Created: 2021-04-09T15:12
tags:
  - Agate
  - Git
  - Devops
type: Article
State: Done
---
# Understanding CI/CD

CI/CD refer to Continuous Integration/Continuous Delivery, (additionally could also include Continuous Deployment). It is a practice that implement automation to building, testing, and deployment stage in software development

![[Untitled 35.png|Untitled 35.png]]

**TL-DR** :

CI/CD is Automatically . . .

- **Continuous Integration** → Build software based on new changes, and test it (might also merge it)
- **Continuous Delivery** → Build software for specific environment, and automatically deliver it to user
- **Continuous Deployment →** Deploy built software into an infrastructure (usually for backend services)

# Gitlab CI/CD

Tool Provided by gitlab to implement ci/cd into gitlab repository. Agate uses gitlab CI/CD for it's CI/CD implementation

## CI/CD Composition

![[pipelines.png]]
### Pipeline

series of interconnected **stage** that represent **entirety** of CI/CD process.

### Stage

Section of pipeline that groups **job** based on their **execution order**. **Stage** represent **ordering** of each automation process.

**example :** in image above, build stage are the first stage that need to be executed, before followed by test, and staging, which finally ended in production stages.

### Job

Job are a task that can be done by a runner, these job can result in multiple way affecting next stage of the pipelines.

### Runner

Runner is a machine that **registered to gitlab CI** with **several identifier** to be assigned **jobs** that suit their identifier.

## Implementing Gitlab CI/CD

Gitlab CI/CD definition initially referred through `.gitlab-ci.yml`.

it read this file every commit and create pipeline jobs based on data insides it.

## Variables in Gitlab CI/CD

Variable in Gitlab is used to add these following features :

- **hide confidential data** that required by the job
- **re-use existing job** by changing some value on it scripts execution
- **specify environment** specific value
- fetch **gitlab related data** (merge id, commit id, pipeline id, job, id)

### Variable Definition

Variable in Gitlab are defined through multiple ways.

1. Gitlab Job Pre-Execution configuration → variable definition in job pre-execution interface
2. **Gitlab ci/cd Variable configuration →** variable definition in gitlab repository ci-cd settings
3. **yaml →** variable definition that exist in `gitlab-ci.yml` files
4. **runner →** variable definition that exist on runner configuration

list above are ordered by definition priority. lower number have higher priority

**Example :**

- **yaml** define variable `AWS_BUCKET = "sample-value"`
- **gitlab ci/cd variable configuration** define `AWS_BUCKET = "agate-aws-bucket"`
- final variable value will be `"agate-aws-bucket"`

### Variable Environment Scope

Variable Environment Scope is a way to define environment specific variable value.

By default Variable Scope are created by gitlab from reading gitlab-ci.yml

  

![[Untitled 1 1.png|Untitled 1 1.png]]

# Job Definition

below are job definition specification

> [!important]  
> these specification is the most you need to change when using pre-made gitlab ci/cd  

```YAML
build-feature:  # job names
	stage: build # stage of the job
  extends: .build-template # job which this job are extending from 
	when: always # job execution timing
  variables: # variable definitions
    GAME_SUFFIX: "-dev"
    BUILD_COMMAND: "build:dev"
  except: # limits when job are not created
    - develop
    - staging
    - master
	only: # limits when job are created
		- tags 
```

> [!info] Keyword reference for the .gitlab-ci.yml file  
> This document lists the configuration options for your GitLab .  
> [https://docs.gitlab.com/ee/ci/yaml/README.html#job-keywords](https://docs.gitlab.com/ee/ci/yaml/README.html#job-keywords)  

more detail on jobs keyword