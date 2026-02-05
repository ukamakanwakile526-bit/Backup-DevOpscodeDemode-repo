     

       pipeline {
    agent none   // Don't use a default node

    tools {
        jdk 'myjava'
        maven 'mymaven'
    }

    stages {

        // Checkout your forked repo once on any available node
        stage('Checkout') {
            agent any
            steps {
                echo 'Cloning forked repo...'
                git url: 'https://github.com/ukamakanwakile526-bit/Backup-DevOpscodeDemode-repo.git', branch: 'master'
            }
        }

        // Compile stage on slave1
        stage('Compile') {
            agent { label 'slave1' }
            steps {
                echo 'Compiling project on slave1...'
                sh 'mvn compile'
            }
        }

        // Code review (PMD) on slave2
        stage('CodeReview') {
            agent { label 'slave2' }
            steps {
                echo 'Running PMD code review on slave2...'
                sh 'mvn pmd:pmd'
            }
        }

        // Unit tests on slave1
        stage('UnitTest') {
            agent { label 'slave1' }
            steps {
                echo 'Running unit tests on slave1...'
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        // Package on any available node
        stage('Package') {
            agent any
            steps {
                echo 'Packaging project...'
                sh 'mvn package'
            }
        }
    }
}
