node {
    def commit_id
    stage('Preparation') {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit_id"
        commit_id = readFile('commit_id').trim()
    }
    stage('test') {
        nodejs(nodeJSInstallationName: 'nodejs') {
            sh 'npm install --only=dev'
            sh 'npm test'
        }
    }
    stage('docker build/push') {
        docker.withRegistry('https://index.docker.io/v1/', 'b8ea2520-2cd6-4800-9fb4-87e45b6944e9') {
            def app = docker.build("bendi89/docker-example:${commit_id}", '.').push()
        }
    }
}