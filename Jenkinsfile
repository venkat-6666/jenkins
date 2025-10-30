pipeline{
    agent any
    stages {
        stage('installing git') {
            steps {
                sh 'apt update && apt install git -y'
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
        stage('Create Dockerfile') {
            steps {
                sh '''
                cat > Dockerfile <<'EOF'
                FROM openjdk:21-slim
                WORKDIR /app
                COPY spring-petclinic/target/spring-petclinic-3.5.0-SNAPSHOT.jar app.jar
                EXPOSE 8080
                CMD ["java", "-jar", "app.jar"]
                EOF
                '''
            }
        }
        stage('docker builiding and running') {
            steps {
                echo 'Building and Running Docker Container'
                sh 'docker build -t petclinic-app .'
                sh 'docker run -d -p 8081:8080 petclinic-app'
            }
        }
    }


}
