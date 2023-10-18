# **Business Central Environments**

## DEV_SANDBOX
Purpose: Development and testing

Link: https://businesscentral.dynamics.com/9b6a813b-7d80-4976-bf5e-10b1b7b5dc88/Sandbox](https://businesscentral.dynamics.com/98a9a56b-05b0-4d5c-95e2-37f22a0b5c6b/SKG-BCS-DEV01/
Repo Branch: test

## TEST_SANDBOX
Purpose: Environment for user acceptance testing

Link: https://businesscentral.dynamics.com/9b6a813b-7d80-4976-bf5e-10b1b7b5dc88/Test](https://businesscentral.dynamics.com/98a9a56b-05b0-4d5c-95e2-37f22a0b5c6b/SKG-BCS-TST01/
Repo Branch: uat

# **Delivery Path**

``` mermaid
graph LR;
    DEV_SANDBOX-->TEST_SANDBOX;
    TEST_SANDBOX-->AppSource;
```

# **Repository Workflow**

```mermaid
sequenceDiagram
    autonumber
    participant feature
    participant test
    participant release
    participant master

    loop New feature development
        master->>feature: create new feature branch
        Note over master,feature: feature branch name format: feature/12345-Task Description <br> where 12345 is GitHub request number

        loop New change/fix implementation
            feature->>feature: Commit(s)
            Note over feature,feature: commit format: #35;12345-Task Description

            feature->>test: merge feature branch to test branch, 
            test->>test: deploy test branch to <br> SANDBOX environment                  
        end
    end

    loop New extension version release
        master->>release: create new release branch
        Note over master,release: release branch name format: release/1.0.0.0

        feature->>release: merge feature branch(es) to release branch
        Note over feature,release: multiple feature brunches can be included into single release branch
        release->>release: deploy release branch <br>to PRODUCTION environment
        release->>release: Commit release
        Note over release: commit format: v1.0.0.0
        release->>master: merge release branch to master

        release->>test: merge release branch to test branch
    end
```
