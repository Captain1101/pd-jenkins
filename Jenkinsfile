pipeline {
    agent any
    triggers{ pollSCM('*/5 * * * *') }
    

    stages {

        // stage('Check-Versions') {
        //     steps {
        //         bat 'node -v'
        //         bat 'npm -v'
        //         bat 'pip --version'
        //         bat 'pm2 --version'
        //     }
        // }

        stage('install-pip-deps') {
            steps {
                script{
                    build()
                }
            }
        }
        //delet
        stage('Check-Versions') {
            steps {
                bat 'node -v'
                bat 'npm -v'
                bat 'pip --version'
                bat 'pm2 --version'
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
    sh "ls -la"
    //sh "npm install -g npm"
    //sh "npm install -g pm2"
    sh "ls"  //delete
    //sh "npm -v" //delete
    //sh "pm2 -v" //delete
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    //sh "ls -la" //delete
    sh "pip install -r requirements.txt"
    //sh "ls -la" //delete
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    //sh "ls" //delete
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    //sh "ls" //delete
    sh "npm -v" //delete
    //sh "pm2 -v" //delete
    //sh "npm init -y"
    //sh "npm install"
    //sh "ls" //delete
    //sh "npm install pm2"
    sh "ls" //delete
    sh "pm2 delete \"greetings-app-${environment}\" & set \"errorlevel=0\""
    sh "pm2 start app.py --name \"greetings-app-${environment}\" -- --port ${port}"
}

def test(String test_set, String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    sh "npm install"
    sh "npm run ${test_set} ${test_set}_${environment}"
}