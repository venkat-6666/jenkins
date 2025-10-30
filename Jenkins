pipeline{
    agent any
    stages {
        stage('installing git') {
            steps {
                sh 'apt-get update && apt-get install git -y'
            }
        }
        stage('cloning repo') {
            steps {
                sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage('building') {
            steps {
                sh 'mvn clean package -DskipTests=true'

            }
        }
        stage('docker build') {
            steps {
                sh 'echo "cat > Dockerfile << 'EOF'
                    FROM openjdk:21-slim
                    RUN apt update -y 
                    RUN apt install maven -y && apt install git -y
                    WORKDIR /app
                    RUN git clone https://github.com/spring-projects/spring-petclinic.git 
                    RUN cd /app/spring-petclinic && mvn package -DskipTests
                    EXPOSE 8080
                    CMD ["java" ,"-jar", "/app/spring-petclinic/target/spring-petclinic-3.5.0-SNAPSHOT.jar"]
                    EOF'
                echo "Building Docker Image"
 
            }
        }
        stage('docker run') {
            steps {
                sh 'docker build -t petclinic-app .'
                sh 'docker run -d -p 8080:8080 petclinic-app'
            }
        }
    }
}
