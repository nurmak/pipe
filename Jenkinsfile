
pipeline {
    agent any
    stages {
        stage('checkout git') {
            steps {
                scm {
        git {
            remote {
                github("git@git.ops.corp.zoom.us:op/web-ansible.git", "ssh")
                credentials("git-ops")
            }
            branch("*/master")
        }
    }
    parameters {
        
        choiceParam("cluster_name", ["None",
                                     "------------------------------------------------",
                                     "go", "go-va2",
                                     "------------------------------------------------",
                                     "aw1", "aw1-ca"
                                     "------------------------------------------------",
                                     "us04"
                                     "----https://web-deploy-us07.corp.zoom.us/-------",
                                     "us05"
                                     "------------------------------------------------",
                                     "us0601"
                                     "------------------------------------------------",
                                     "eu01"
                                     "------------------------------------------------",
                                     "au01"
                                     "_________________________________________________"],
                                     "cluster")

            }
        }

        stage('build for Single') {
            steps {
                sh 'Running Ansible job'
            }
        }

        stage ('test Single') {
            steps {
                parallel (
                    "TA test Single": { sh 'TA' },
                    "integration tests": { sh 'integration-test' }
                )
            }
        }

        stage('Build for VA'){
            steps {
                deploy(developmentServer, serverPort)
            }
        }

        stage('Build for VA2'){
            steps {
                deploy(stagingServer, serverPort)
            }
        }

        stage ('TA test for whole Cluster') {
            steps {
                parallel (
                    "TA test Single": { sh 'TA' },
                    "integration tests": { sh 'integration-test' }
                )
            }
        }

        stage('deploy production'){
            steps {
                deploy(productionServer, serverPort)
            }
        }
    }
    post {
        failure {
            mail to: 'team@example.com', subject: 'Pipeline failed', body: "${env.BUILD_URL}"
        }
    }
}
