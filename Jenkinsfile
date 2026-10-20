pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build WAR') {
            steps {
                sh '''
                    export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-21.0.12.1.1-1.2.el9.x86_64
                    export PATH="$JAVA_HOME/bin:$PATH"

                    java --version
                    mvn --version
                    mvn clean package
                    test -s target/student-result.war
                    ls -lh target/student-result.war
                '''
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/student-result.war',
                                 fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'sudo -n /usr/local/sbin/deploy-student-result.sh'
            }
        }
    }

    post {
        success {
            echo 'Build and Tomcat deployment completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output for the failed stage.'
        }
    }
}
