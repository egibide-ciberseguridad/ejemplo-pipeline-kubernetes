pipeline {
    agent {
        kubernetes {
            serviceAccount 'jenkins-99'
            yamlFile 'manifests/despliegue.yml'
        }
    }
    stages {
        stage('Ver donde estamos') {
            steps {
                container('hugo') {
                    script {
                        sh 'pwd'
                        sh 'ls -l'
                    }
                }
            }
        }
        stage('Compilar la web') {
            steps {
                container('hugo') {
                    script {
                        sh 'cp -f ./www/index.html /web/index.html'
                        sh 'sed -i "s/{{ FECHA }}/$(date)/g" /web/index.html'
                    }
                }
            }
        }
        stage('Desplegar el pod') {
            steps {
                container('kubectl') {
                    script {
                        sh 'kubectl delete pod ejemplo-pipeline-kubernetes --ignore-not-found'
                        sh 'kubectl apply -f manifests/pod.yml'
                    }
                }
            }
        }
    }
}
