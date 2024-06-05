pipeline {
    agent any 

    stages {
        stage('Checkout from Git') {
            steps {
                git url: 'https://github.com/Tanya707/RepotPortal.git', branch: 'main'
            }
        }
        stage('Clean') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat 'dotnet clean ReportPortal.sln'
                }
            }
        }
        stage('Build') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat 'dotnet build ReportPortal.sln -c Debug'
                }
            }
        }
        stage('Run NUnit Tests') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat 'dotnet test UI.Tests.NUnit\\UI.Tests.NUnit.csproj --logger "trx;LogFileName=nunit_results.trx"'
                }
            }
        }
        stage('Run MSTest Tests') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat 'dotnet test UI.Tests.MSTests\\UI.Tests.MSTests.csproj --logger "trx;LogFileName=mstest_results.trx"'
                }
            }
        }
        stage('Run NUnit Tests for API.Business') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat 'dotnet test API.Tests\\API.Tests.csproj --logger "trx;LogFileName=API.Tests_results.trx"'
                }
            }
        }
    }

    post {
        always {
            script {
                bat 'dir /S /B *results.trx'
            }
            junit '**/UI.Tests.NUnit/TestResults/nunit_results.trx'
            mstest testResultsFile: '**/UI.Tests.MSTests/TestResults/mstest_results.trx'
            junit '**/API.Tests/TestResults/API.Tests_results.trx'
        }
    }
}
