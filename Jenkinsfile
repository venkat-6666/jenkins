pipeline{
    agent any
    stages {
        stage('installing git') {
            steps {
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y git maven
               '''

            }
        }
        stage('cloning repo') {
            steps {
                sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
                dir('spring-petclinic') {
                    sh 'mvn clean verify -DskipTests=true'
            }
         }
        }
        stage('Create Dockerfile') {
            steps {
                dir('spring-petclinic') {
                sh '''
                cat > Dockerfile <<'EOF'
                FROM openjdk:21-slim
                WORKDIR /app
                COPY ./target/spring-petclinic-3.5.0-SNAPSHOT.jar app.jar
                EXPOSE 8080
                CMD ["java", "-jar", "app.jar"]
                EOF
                '''
             }
         }
        }
        stage('docker builiding and running') {
            steps {
                echo 'Building and Running Docker Container'
                sh 'docker build -t petclinic-app .'
                sh 'docker run -d -p 8080:8080 petclinic-app'
            }
        }
    


}
}
