# jenkins

### Pipeline
A continuous delivery (CD) pipeline is an automated expression of your process for getting software from version control right through to your users and customers. Every change to your software (committed in source control) goes through a complex process on its way to being released. This process involves building the software in a reliable and repeatable manner, as well as progressing the built software (called a "build") through multiple stages of testing and deployment.
The definition of a Jenkins Pipeline is written into a text file (called a Jenkinsfile) which in turn can be committed to a project’s source control repository. This is the foundation of "Pipeline-as-code"; treating the CD pipeline as a part of the application to be versioned and reviewed like any other code.

[!preview] (https://www.jenkins.io/doc/book/resources/pipeline/realworld-pipeline-flow.png)

#### Pipeline
A Pipeline is a user-defined model of a CD pipeline. A Pipeline’s code defines your entire build process, which typically includes stages for building an application, testing it and then delivering it.
#### Node
A node is a machine which is part of the Jenkins environment and is capable of executing a Pipeline.
#### Stage
A stage block defines a conceptually distinct subset of tasks performed through the entire Pipeline (e.g. "Build", "Test" and "Deploy" stages), which is used by many plugins to visualize or present Jenkins Pipeline status/progress.
#### Step
A single task. Fundamentally, a step tells Jenkins what to do at a particular point in time (or "step" in the process). For example, to execute the shell command make, use the sh step: sh 'make'. When a plugin extends the Pipeline DSL, [1] that typically means the plugin has implemented a new step.

Jenkinsfile (Declarative Pipeline)
```
pipeline {
    agent any
  //Execute this Pipeline or any of its stages, on any available agent.
    stages {
        stage('Build') {
          //	Defines the "Build" stage.
            steps {
                // Perform some steps related to the "Build" stage.
            }
        }
        stage('Test') {
            // Defines the "Test" stage.
            steps {
                // Perform some steps related to the "Test" stage.
            }
        }
        stage('Deploy') {
            // Defines the "Deploy" stage.
            steps {
                // Perform some steps related to the "Deploy" stage.
            }
        }
    }
}
```

Jenkinsfile (Scripted Pipeline)
```
node {  //Execute this Pipeline or any of its stages, on any available agent.
    stage('Build') { // Defines the "Build" stage. stage blocks are optional in Scripted Pipeline syntax. However, implementing stage blocks in a Scripted Pipeline provides clearer visualization of each stage's subset of tasks/steps in the Jenkins UI.
        // Perform some steps related to the "Build" stage.
    }
    stage('Test') { // Defines the "Test" stage.
        // Perform some steps related to the "Test" stage.
    }
    stage('Deploy') { // Defines the "Deploy" stage.
        // Perform some steps related to the "Deploy" stage.
    }
}
```


example -Scripted Pipeline 
```
node {
  stage('hello') {
      echo "hello"
    }
}
```

example - Declarative Pipeline
```
pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!'
            }
        }
    }
}
```

ex-2
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```
### environment variables
Jenkins Pipeline exposes environment variables via the global variable env, which is available from anywhere within a Jenkinsfile. The full list of environment variables accessible from within Jenkins Pipeline is documented at ${YOUR_JENKINS_URL}/pipeline-syntax/globals#env
#### BUILD_ID
The current build ID, identical to BUILD_NUMBER for builds created in Jenkins versions 1.597+

#### BUILD_NUMBER
The current build number, such as "153"

#### BUILD_TAG
String of jenkins-${JOB_NAME}-${BUILD_NUMBER}. Convenient to put into a resource file, a jar file, etc for easier identification

#### BUILD_URL
The URL where the results of this build can be found (for example http://buildserver/jenkins/job/MyJobName/17/ )

#### EXECUTOR_NUMBER
The unique number that identifies the current executor (among executors of the same machine) performing this build. This is the number you see in the "build executor status", except that the number starts from 0, not 1

#### JAVA_HOME
If your job is configured to use a specific JDK, this variable is set to the JAVA_HOME of the specified JDK. When this variable is set, PATH is also updated to include the bin subdirectory of JAVA_HOME

#### JENKINS_URL
Full URL of Jenkins, such as https://example.com:port/jenkins/ (NOTE: only available if Jenkins URL set in "System Configuration")

#### JOB_NAME
Name of the project of this build, such as "foo" or "foo/bar".

#### NODE_NAME
The name of the node the current build is running on. Set to 'master' for the Jenkins controller.

#### WORKSPACE
The absolute path of the workspace

Referencing or using these environment variables can be accomplished like accessing any key in a Groovy Map, for example:
```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
    }
}
```

Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
                sh 'printenv'
            }
        }
    }
}





