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
                sh 'docker run -d -p 8080:8080 petclinic-app'
            }
        }
    }


}


/////////////////////////////////////


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
                sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build Application') {
            steps {
                dir('spring-petclinic') {
                    sh 'mvn clean package -DskipTests=true'
                }
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

        stage('Docker Build') {
            steps {
                sh '''
                    echo "Building Docker Image..."
                    docker build -t petclinic-app .
                '''
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    echo "Running Docker Container..."
                    docker run -d -p 8080:8080 --name petclinic petclinic-app
                '''
            }
        }
    }
}
