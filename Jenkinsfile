def deployApp(String environment, String port) {
    def workspace = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\python-greetings-pipeline"
    bat "set HOMEPATH=\\Users\\maksi && set PM2_HOME=C:\\Users\\maksi\\.pm2 && \"C:\\Users\\maksi\\AppData\\Roaming\\npm\\pm2.cmd\" delete greetings-app-${environment} & EXIT /B 0"
    bat "if exist venv rmdir /s /q venv"
    bat "\"C:\\Program Files\\Odoo 19.0.20260311\\python\\python.exe\" -m venv venv"
    bat "venv\\Scripts\\python.exe -m pip install -r requirements.txt"
    bat "set HOMEPATH=\\Users\\maksi && set PM2_HOME=C:\\Users\\maksi\\.pm2 && set PORT=${port} && \"C:\\Users\\maksi\\AppData\\Roaming\\npm\\pm2.cmd\" start app.py --name greetings-app-${environment} --interpreter \"${workspace}\\venv\\Scripts\\python.exe\""
}

def runTests(String environment) {
    bat "npm install"
    bat "npm run greetings -- greetings_${environment}"
}

pipeline {
    agent any
    stages {
        stage('install-pip-deps') {
            steps {
                echo "Installing all required dependencies..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings']])
                bat "\"C:\\Program Files\\Odoo 19.0.20260311\\python\\python.exe\" -m venv venv"
                bat "venv\\Scripts\\python.exe -m pip install -r requirements.txt"
            }
        }
        stage('deploy-to-dev') {
            steps {
                echo "Deploying application to DEV environment on port 7001..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings']])
                script { deployApp('dev', '7001') }
            }
        }
        stage('tests-on-dev') {
            steps {
                echo "Running automated tests on DEV environment..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework']])
                script { runTests('dev') }
            }
        }
        stage('deploy-to-stg') {
            steps {
                echo "Deploying application to STG environment on port 7002..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings']])
                script { deployApp('stg', '7002') }
            }
        }
        stage('tests-on-stg') {
            steps {
                echo "Running automated tests on STG environment..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework']])
                script { runTests('stg') }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                echo "Deploying application to PREPROD environment on port 7003..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings']])
                script { deployApp('preprod', '7003') }
            }
        }
        stage('tests-on-preprod') {
            steps {
                echo "Running automated tests on PREPROD environment..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework']])
                script { runTests('preprod') }
            }
        }
        stage('deploy-to-prod') {
            steps {
                echo "Deploying application to PROD environment on port 7004..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings']])
                script { deployApp('prod', '7004') }
            }
        }
        stage('tests-on-prod') {
            steps {
                echo "Running automated tests on PROD environment..."
                checkout scmGit(branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework']])
                script { runTests('prod') }
            }
        }
    }
}