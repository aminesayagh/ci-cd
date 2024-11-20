
# Git Diagram

```mermaid
classDiagram
    class Repository {
        +Contains branches
        +Contains tags
        +Has configuration
        +Initialize()
        +Clone()
        +Fetch()
        +Push()
        +Pull()
    }
    class Branch {
        +Points to latest commit
        +Contains commits
        +Has HEAD reference
        +Create()
        +Delete()
        +Checkout()
        +Merge()
        +Rebase()
    }
    class Commit {
        +Has parent commit(s)
        +Contains changes
        +Has metadata
        +author: string
        +date: timestamp
        +message: string
        +Create()
        +Revert()
        +Cherry-pick()
    }
    class WorkingDirectory {
        +Contains edited files
        +Shows file status
        +Tracks changes
        +Status()
        +Clean()
        +Reset()
        +Stash()
    }
    class StagingArea {
        +Holds staged changes
        +Tracks file status
        +Index management
        +Add()
        +Remove()
        +Reset()
    }
    class Localepository {
        +Mirrors remote repository
        +Has URL
        +Has credentials
        +Push()
        +Fetch()
        +Clone()
    }
    class Tag {
        +References specific commit
        +Has name
        +Has message
        +Create()
        +Delete()
        +Push()
    }
    class HEAD {
        +Points to current position
        +References branch/commit
        +Can be detached
        +Move()
        +Detach()
    }
    class MergeConflict {
        +Requires resolution
        +Affects files
        +Shows conflict markers
        +Resolve()
        +Abort()
        +List conflicts()
    }
    class PullRequest {
        +Proposes changes
        +Has description
        +Has reviews
        +Create()
        +Review()
        +Merge()
        +Close()
    }
    class GitConfig {
        +User settings
        +Repository settings
        +Global settings
        +Get()
        +Set()
        +List()
    }
    class GitIgnore {
        +Pattern rules
        +Exclusions
        +Update()
        +Check file()
    }

    Repository "1" --> "*" Branch : contains
    Repository "1" --> "*" Tag : contains
    Repository "1" --> "0..n" RemoteRepository : connected to
    Repository "1" --> "1" GitConfig : has
    Repository "1" --> "1" GitIgnore : contains
    Branch "1" --> "1" HEAD : has current
    Branch "1" --> "*" Commit : contains
    Branch "1" --> "0..n" Branch : merges with
    Commit "1" --> "0..2" Commit : has parent
    WorkingDirectory "1" --> "1" StagingArea : stages changes
    StagingArea "1" --> "1" Commit : creates
    RemoteRepository "1" <--> "1" Repository : sync via push/pull
    Tag "1" --> "1" Commit : references
    MergeConflict "0..n" --> "1" WorkingDirectory : resolved in
    PullRequest "0..n" --> "1..2" Branch : connects
    GitIgnore "1" --> "1" WorkingDirectory : filters
    HEAD "1" --> "1" Commit : points to
    WorkingDirectory "1" --> "1" GitIgnore : uses
```

```mermaid
@startuml
' Define classes
class JenkinsMaster {
  - name: String
  - version: String
  + manageJobs()
  + scheduleJobs()
  + distributeTasks()
}

class JenkinsAgent {
  - name: String
  - OS: String
  - labels: String[]
  + executeJob()
  + reportStatus()
}

class Executor {
  - id: String
  - status: String
  + runStep()
  + updateStatus()
}

class Job {
  - name: String
  - type: String
  - status: String
  + defineWorkflow()
  + maintainBuildHistory()
}

class Pipeline {
  - script: String
  - stages: Stage[]
  + automateProcess()
  + visualizeWorkflow()
}

class Stage {
  - name: String
  - steps: Step[]
  + organizeTasks()
  + manageDependencies()
}

class Step {
  - name: String
  - command: String
  + executeAction()
}

class Jenkinsfile {
  - path: String
  + definePipeline()
}

class Plugin {
  - name: String
  - version: String
  + extendFunctionality()
  + integrateTools()
}

class BuildQueue {
  - queue_id: String
  - job: Job
  + enqueueJob()
  + dequeueJob()
}

class Artifact {
  - name: String
  - path: String
  + archive()
  + retrieve()
}

class Credential {
  - id: String
  - type: String
  + store()
  + retrieve()
}

' Define relationships
JenkinsMaster "1" --> "*" JenkinsAgent : manages >
JenkinsAgent "1" --> "*" Executor : runs >
Executor "1" --> "1" Step : executes >
Job "1" --> "1" Pipeline : defines >
Pipeline "1" --> "*" Stage : contains >
Stage "1" --> "*" Step : includes >
JenkinsMaster "1" --> "*" Job : schedules >
JenkinsMaster "1" --> "*" BuildQueue : manages >
BuildQueue "1" --> "1" Job : queues >
Job "1" --> "*" Artifact : produces >
Job "1" --> "*" Credential : uses >
Pipeline "1" --> "1" Jenkinsfile : defined by >
JenkinsMaster "1" --> "*" Plugin : utilizes >
Plugin "1" --> "*" JenkinsAgent : extends >
JenkinsMaster "1" --> "*" Credential : manages >
@enduml
```
