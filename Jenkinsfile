       pipeline {
    agent none

    tools {
        jdk 'myjava'
        maven 'mymaven'
    }

    stages {

        stage('Checkout') {
            agent any
            steps {
                echo 'Cloning forked repo...'
                git url: 'https://github.com/ukamakanwakile526-bit/Backup-DevOpscodeDemode-repo.git', branch: 'master'

            }
        }

        stage('Compile with slave1') {
            agent { label 'slave1' }
            steps {
                echo 'Compiling...'
                sh 'mvn compile'
            }
        }

        stage('CodeReview with slave2') {
            agent { label 'slave2' }
            steps {
                echo 'Code review...'
                sh 'mvn pmd:pmd'
            }
        }

        stage('UnitTest with slave1') {
            agent { label 'slave1' }
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            agent any
            steps {
                echo 'Packaging...'
                sh 'mvn package'
            }
        }
    }
}
