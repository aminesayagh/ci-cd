
# Git Diagram

```mermaid
classDiagram
    class Repository {
        +Contains branches
        +Contains tags
        +Has configuration
        +Initialize()
        +Clone()
    }
    class RemoteRepository {
        +Serves as central repository
        +Accepts push/pull
        +Handles authentication
        +Tracks all branches
    }
    class LocalRepository {
        +Mirrors remote repository
        +Contains complete history
        +Manages local branches
        +Push()
        +Pull()
        +Fetch()
    }
    class Branch {
        +Points to latest commit
        +Contains commits
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
        +hash: string
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
    class Tag {
        +References specific commit
        +Has name
        +Has message
        +Create()
        +Delete()
        +Push()
    }
    class HEAD {
        +Points to current branch
        +References current commit
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

    %% Core Repository Structure
    RemoteRepository "1" <--> "0..n" LocalRepository : sync via push/pull
    LocalRepository "1" --> "*" Branch : contains
    LocalRepository "1" --> "*" Tag : contains
    LocalRepository "1" --> "1" GitConfig : has
    LocalRepository "1" --> "1" GitIgnore : contains

    %% Branch and Commit Structure
    Branch "1" --> "1" HEAD : points to current
    Branch "1" --> "*" Commit : contains
    Branch "1" --> "0..n" Branch : merges with
    Commit "1" --> "0..2" Commit : has parent

    %% Working Directory Flow
    WorkingDirectory "1" --> "1" StagingArea : stages changes
    StagingArea "1" --> "1" Commit : creates
    LocalRepository "1" --> "1" WorkingDirectory : workspace
    
    %% References and Tags
    Tag "1" --> "1" Commit : references
    HEAD "1" --> "1" Branch : points to
    HEAD "1" --> "1" Commit : references when detached

    %% Workflow Elements
    MergeConflict "0..n" --> "1" WorkingDirectory : resolved in
    PullRequest "0..n" --> "1..2" Branch : connects
    GitIgnore "1" --> "1" WorkingDirectory : filters
    
    %% Config and Settings
    GitConfig "1" --> "1" LocalRepository : configures
    GitIgnore "1" --> "1" LocalRepository : filters for
```

# Jenkins

```mermaid

classDiagram
    class JenkinsMaster {
        +Manage jobs and agents
        +Schedule builds
        +Handle security
        +Configure system
        +Distribute tasks
        +Monitor system health
    }
    class JenkinsAgent {
        +name: string
        +status: string
        +labels: array
        +Execute builds
        +Run tests
        +Deploy applications
    }
    class Pipeline {
        +stages: array
        +environment: dict
        +Define workflow
        +Execute stages
        +Handle failures
        +Generate reports
    }
    class Stage {
        +name: string
        +steps: array
        +Execute steps
        +Check conditions
        +Report status
    }
    class Job {
        +name: string
        +type: string
        +Configure build steps
        +Set triggers
        +Manage workspace
        +Store artifacts
    }
    class BuildQueue {
        +pending jobs: array
        +Schedule builds
        +Manage priorities
        +Handle dependencies
    }
    class Workspace {
        +path: string
        +Store build files
        +Clean workspace
        +Manage artifacts
    }
    class Jenkinsfile {
        +pipeline script
        +Define stages
        +Set environment
        +Configure triggers
    }
    class Plugin {
        +name: string
        +version: string
        +Extend functionality
        +Add integrations
    }
    class Artifact {
        +name: string
        +path: string
        +Store build outputs
        +Archive results
    }
    class Credential {
        +id: string
        +type: string
        +Secure access
        +Manage secrets
    }

    %% Core Relationships
    JenkinsMaster "1" --> "*" JenkinsAgent : manages
    JenkinsMaster "1" --> "*" Job : orchestrates
    JenkinsMaster "1" --> "1" BuildQueue : uses
    
    %% Job and Pipeline Relations
    Job "1" --> "1" Pipeline : defines
    Job "1" --> "1" Workspace : uses
    Job "1" --> "*" Artifact : produces
    
    %% Pipeline Structure
    Pipeline "1" --> "*" Stage : contains
    Pipeline "1" --> "1" Jenkinsfile : defined in
    
    %% Agent Relations
    JenkinsAgent "1" --> "*" Workspace : maintains
    JenkinsAgent "1" --> "*" Pipeline : executes
    
    %% Security and Extensions
    JenkinsMaster "1" --> "*" Credential : manages
    JenkinsMaster "1" --> "*" Plugin : uses
    Job "1" --> "*" Credential : uses
```
```yaml
// Jenkinsfile

pipeline {
    agent any

    environment {
        // Define environment variables or credentials
        DEPLOY_ENV = credentials('deploy-env-credential-id')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm install'
                sh 'npm run build'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
            post {
                always {
                    junit 'reports/**/*.xml'
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying to ${env.DEPLOY_ENV} environment..."
                sh './deploy.sh --env ${env.DEPLOY_ENV}'
            }
        }
    }

    post {
        failure {
            mail to: 'team@example.com',
                 subject: "Jenkins Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.JOB_NAME}.\nPlease check the Jenkins console output for details."
        }
    }
}
```
# GitHub Action

```mermaid
classDiagram
    class Workflow {
        +name: string
        +on: trigger
        +Contains jobs
        +YAML definition
        +Environment variables
    }
    class Job {
        +name: string
        +runs-on: runner
        +Contains steps
        +needs: dependencies
        +conditions
    }
    class Step {
        +name: string
        +uses: action
        +run: commands
        +with: inputs
        +env: variables
    }
    class Runner {
        +OS type
        +Environment
        +Execute jobs
        +Handle secrets
    }
    class Action {
        +name: string
        +inputs
        +outputs
        +Reusable logic
    }
    class Event {
        +push
        +pull_request
        +schedule
        +manual trigger
    }
    class Artifact {
        +name: string
        +path: string
        +retention period
        +Upload()
        +Download()
    }
    class Secret {
        +name: string
        +value: encrypted
        +scope: repository/org
    }

    %% Core relationships
    Event "1" --> "*" Workflow : triggers
    Workflow "1" --> "1..*" Job : contains
    Job "1" --> "1..*" Step : contains
    Job "1" --> "1" Runner : runs on
    Step "0..1" --> "1" Action : uses
    Job "1" --> "*" Artifact : produces
    Job "1" --> "*" Secret : uses
    Step "1" --> "*" Secret : uses
```
