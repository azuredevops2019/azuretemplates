pipeline {
    agent any

   environment {
        APP_ID = ""
        PASSWORD = ""
   }

   options {
    timestamps()
    disableConcurrentBuilds()
   }

   parameters {
        booleanParam(name: 'CREATE_RS', defaultValue: false, description: '')
        booleanParam(name: 'USE_EXISTED_RS', defaultValue: false, description: '')
        string(name: 'rs_name')
        string(name: 'tag_LOB')
        string(name: 'tag_EnvType')
        string(name: 'tag_SenType')
        string(name: 'tag_Deployer')
        string(name: 'tag_DeployDate')
        choice(name: 'Template', choices: ['one', 'two', 'three'], description: '')
   }

   stages {
        stage("Create a new Resource Group") {
            steps {
                sh 'az group create -n ${params.rs_name} -l "westeurope" --tags LOB="${params.tag_LOB}" EnvType="${params.tag_EnvType}" SenType="${params.tag_SenType}" Deployer="${params.tag_Deployer}" DeployDate="${params.tag_DeployDate}"'
            }
        }

        stage("Use an existed Resource Group") {
            steps {
                sh 'echo 0'
            }
        }

        stage("Login with Service Principle") {
            steps {
                sh 'az login --service-principal -u ${env.APP_ID} -p ${env.PASSWORD} --tenant a26958e2-7133-4895-9343-3c2abb69b942'
            }
        }

        stage("Deploy template") {
            steps {
                sh 'az group deployment create  --name "${params.TemplateName}" --resource-group "${params.rs_name}" --template-file "${params.TemplateName}.json" --parameters @"${params.TemplateName}.parameters.json"'
            }
        }

   }
}