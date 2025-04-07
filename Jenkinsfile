pipeline {
    agent any
    triggers{ pollSCM('*/5 * * * *') }
    

    stages {
        stage('install-pip-deps') {
            steps {
                script{
                    build()
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script{
                    deploy("dev", 7001)
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script{
                    test("greetings", "dev")
                }
            }
        }
        
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("staging", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    test("greetings", "staging")
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("preprod", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    test("greetings", "preprod")
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("prod", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    test("greetings", "prod")
                }
            }
        }
    }
}


def build(){
    echo "Installing all required depdendencies.."
    sh "ls"  //delete
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    sh "pip install -r requirements.txt"
    sh "npm install"
    sh "npm test"
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    sh "npm -v" //delete
    //sh "npm install pm2"
    //sh "pm2 -v" //delete
    sh "ls" //delete
    sh "pm2 delete \"greetings-app-${environment}\" & set \"errorlevel=0\""
    bat "pm2 start app.py --name \"greetings-app-${environment}\" -- -- --port ${port}"
}

def test(String test_set, String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    sh "npm install"
    sh "npm run ${test_set} ${test_set}_${environment}"
}