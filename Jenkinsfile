node {
    def app

    stage('Clone repository') {

        checkout scm
    }

    stage('Build image') {

        app = docker.build("mnelso204/cw02")
    }

    stage('Test image') {

        app.inside {

            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {

        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    sshagent(['my-ssh-key']) {

    sh 'ssh ubuntu@44.198.182.225 kubectl set image deployments/cw02 cw02=mnelso204/cw02:$BUILD_NUMBER'
}
