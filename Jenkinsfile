pipeline {
    agent any
    stages {
        stage('checkout git') {
            steps {
                scm {
        git {
            remote {
                github("https://github.com/nurmak/pipe/new/main", "ssh")
               
            }
            branch("*/main")
        }
    }
}              
