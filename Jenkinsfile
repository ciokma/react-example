
//Variables
def artifactName = ""
def version = ""
def component_group =""

pipeline {

    agent any
   
    environment {

     
        //PROJECT
        MAVEN_PATH = '/opt/jenkins_tools/apache-maven-3.6.0/bin'
        JAVA_PATH= '/usr/lib/jvm/java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64'

        //NEXUS
        NEXUS_URL = '54.226.116.92:8081'
        REPOSITORY = "demo-repo"


    }
    stages {
        stage('Prepare') {
            steps {
                //Use maven to delete unnecesary files from other builds
                echo 'prepare'
                script{

                     // Leer el contenido del archivo package.json
                    def packageJsonContent = sh(script: 'cat package.json', returnStdout: true).trim()

                    // Analizar el contenido JSON
                    def packageJson = readJSON text: packageJsonContent

                    version = env.BUILD_NUMBER
                    artifactName = packageJson.name
              
                    component_group = "demo"


                    echo "${version}"
                    echo "${artifactName}"
                }

            }
        }
        stage('Instalar dependencias de Node.js') {
            steps {
                script {
                    sh 'npm config get registry'
                    sh 'npm config set registry http://54.226.116.92:8081/repository/npm-group/'
                    sh 'npm config get registry'
                }
            }
        }
        stage('Build') {
            steps {

                    sh 'node --version'
                    sh 'npm --version'
                    sh 'npm install' // Instala las dependencias
                    sh 'npm run build' // Compila el código React

                    // Comprime la carpeta del proyecto en un archivo ZIP
                    sh "zip -r ${artifactName}.zip build"

            }
        }
 
        
  
    }
    
 
}
