node {
    def app
    stage('Clone repository') {
        git 'https://github.com/KimGeunHye21/Wimb_DevOps.git'
    }
    stage('Build image') {
        app = docker.build("kimgeunhye21/wimb")
    }
     stage('Test image') {
         app.inside{
         // sh'make test'
         }
     }
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'kimgeunhye21') {
           app.push("${env.BUILD_NUMBER}")
           app.push("latest")
        }
    }
}
