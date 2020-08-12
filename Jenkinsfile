pipeline{
    tools{
        jdk 'my_java'
        maven 'my_maven'
    }
    agent none
        stages{
           stage('checkout'){
           agent any
           steps{
               git 'https://github.com/nageshcena/my-app.git'
           }
        }
        stage('compile'){
            agent any
            steps{
                sh 'mvn compile'
            }
        }
        stage('codeReview'){
            agent any
            steps{
                sh 'mvn pmd:pmd'
            }
            post{
                always{
                    pmd pattern: 'target/pmd.xml'
                }
            }
        }
        stage('UnitTest'){
            agent any
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'mvn test'
                }
            }
        }
        stage('MetricCheck'){
            agent  any
            steps{
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
            post{
                always{
                    cobertura coberturaReportFile:'**/target/site/cobertura/coverage.xml'
                }
            }
           
        }
        stage('Pakcage'){
          agent any
            steps{
                sh 'mvn package'
            }
        }
    }
}
