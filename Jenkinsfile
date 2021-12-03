@Library('piper-lib-os') _

pipeline {
    agent any
    stages {
    stage ('Setup') {
        steps {
            setupCommonPipelineEnvironment script: this
        } // steps
    } //stage
        stage ('Deploy Commit') {
            steps {
                gctsDeploy script: this
            } //steps
        } // stage
        stage ('Execute UnitTest') {
            steps {
                script {
                    checks_failed = false
                    try {
                        gctsExecuteABAPUnitTests script: this
                    } catch (err) {
                        unstable ('UnitTest failed!')
                        checks_failed = true
                    } //catch
                } //script 
            } //steps
        } //stage
        stage ('Rollback') {
            when { expression { checks_failed == true } }
            steps {
                gctsRollback script: this
            } //steps
        } //stage
        stage ('Build success') {
            when { expression { checks_failed == false } }
            steps {
                echo 'Build success'
            } //steps
        } //stage
    } //stages
} //pipeline
