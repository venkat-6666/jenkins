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
                  COPY spring-petclinic/target/spring-petclinic-4.0.0-SNAPSHOT.jar app.jar
                  EXPOSE 8080
                  CMD ["java", "-jar", "app.jar"]
                  EOF
          '''
    }
}

             }
         }
        }
        stage('docker builiding and running') {
            steps {
                dir('spring-petclinic') {
                echo 'Building and Running Docker Container'
                sh 'docker build -t petclinic-app .'
                sh 'docker run -d -p 8080:8080 petclinic-app'
            }
    }
}
