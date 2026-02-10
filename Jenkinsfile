pipeline {
    agent any

    tools {
        maven 'maven-3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean validate'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Benchmark') {
            steps {
                sh '''
                  mkdir -p build/reports/jmh
                  java -jar target/benchmarks.jar \
                    -wi 5 -i 10 -f 2 \
                    -rf json -rff build/reports/jmh/result.json
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/reports/jmh/*.json', fingerprint: true
        }
    }
}