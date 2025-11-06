pipeline {
    agent any

    stages {
        stage('Install Git & Maven') {
            steps {
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y git maven
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                sh 'rm -rf spring-petclinic'
                sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
                dir('spring-petclinic') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Create Dockerfile') {
            steps {
                dir('spring-petclinic') {
                    sh '''
                        cat <<EOF > Dockerfile
FROM openjdk:25-slim
WORKDIR /app
COPY target/spring-petclinic-4.0.0-SNAPSHOT.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
EOF
                    '''
                }
            }
        }

        stage('Build and Run Docker Image') {
            steps {
                dir('spring-petclinic') {
                    echo 'Building and Running Docker Container...'
                    sh 'sudo docker build -t petclinic-app .'
                    sh 'sudo docker run -d -P 8089:8080 petclinic-app'
                }
            }
        }
    }
}
