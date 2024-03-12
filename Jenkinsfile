pipeline {

    parameters {
        booleanparam(name: 'autoapprove', defaultValue: false, description: "Automatically run apply after generating plan?")
    }
    environment {
        AWS_ACCESS_KEY_ID = credentials("AWS_ACCESS_KEY_ID")
        AWS_SECRET_ACCESS_ID = credentials("AWS_SECRET_ACCESS_ID")
        }
    agent any
     stages {
        stage ("checkout") {
            steps {
                script{
                      dir("terraform")
                      {
                        git@github.com:ASFIASHAIKH/PROJECT_02.git
                      }
                }
            }
        }
     }
        stage("plan"){
            steps{
                sh 'pwd;cd terraform/ ; terraform init'
                sh 'pwd;cd terraform/ ; terraform plan -out tfplan'
                sh 'pwd;cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }
        stage("Approval") {
            when {
                not {
                    equals expected: true, actual: params.autoApprove
                }
            }
        }
            steps {
               script {
                    def plan = readFile "terraform/tfplan.txt"
                    input message: "Do you want to apply the plan?" ,
                    parameters: [text(name: 'plan' , description: "Please review the plan" , defaultValue: "plan")]
               }
            }

            stage('Apply') {
                steps {
                    sh "pwd;cd terraform/ ; terraaform apply -input-false tfplan"
                }
            }
}