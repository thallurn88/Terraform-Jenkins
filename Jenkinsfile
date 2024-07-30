pipeline {

    parameters {
        booleanParam(name: 'autoapprove', defaultValue: false, description: "apply the changes once the plan completed?")
    }

    environment {
        secret_Key = credentials("secret_key_ID")
        access_Key = credentials("access_key_ID")
    }

    agent any

    stages {

        stage("Checkout the terraform config") {
            steps {
                git url: 'https://github.com/thallurn88/Terraform-Jenkins.git'
            }
        }

        stage("Plan") {
            steps {
                sh 'terraform init'
                sh 'terraform plan -out tfplan'
                sh 'terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage("Approve") {
            when {
                not {
                    equals expected: true, actual: params.autoapprove
                }
            }
            steps {
                script {
                    def plan = readFile 'tfplan.txt'
                    input message: "Do you want to apply the changes?",
                          parameters: [text(name: 'plan', defaultValue: plan, description: "Please review the plan")]
                }
            }
        }

        stage("Apply") {
            steps {
                sh "terraform apply -input=false tfplan"
            }
        }
    }
}
