node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
       app = docker.build("ilievaana/kiii-jenkins")
    }

    stage('Push image') {   
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
        }
    }

    stage('Deploy') {
        // Update Dockerfile copy command based on the location of index.html
        def dockerfilePath = "${env.WORKSPACE}/jenkins/jenkins-data/workspace/kiii-jenkins_dev/Dockerfile"
        sh "sed -i 's|COPY index.html /usr/share/nginx/html/index.html|COPY ${env.WORKSPACE}/jenkins/jenkins-data/workspace/kiii-jenkins_dev/index.html /usr/share/nginx/html/index.html|' ${dockerfilePath}"
    }
}
