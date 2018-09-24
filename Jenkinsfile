node{
    try{
        def commit_id;
        def image_name;
        stage('preperation'){
            checkout scm
             sh "git rev-parse --short HEAD > .git/commit-id"                        
             commit_id = readFile('.git/commit-id').trim()
        }
        stage('build project'){
            nodejs(nodeJSInstallationName:'nodejs'){
                sh 'npm install'
                sh 'npm run-script build'
            }
        }
        stage('docker build/push'){
              docker.withRegistry('https://index.docker.io/v1/','dockerhub') {
              def app = docker.build("hmttelange/angular:${commit_id}", '.').push()
             }
        }
        stage('remove docker image'){
            sh("docker rmi hmttelange/angular:$commit_id")

        }
        stage('send build mail'){
            emailext  attachLog: true, subject: "${JOB_NAME}-Jenkins built number # ${BUILD_NUMBER} is success", to: 'hanmantmtelange@gmail.com,hmt4275@gmail.com'
        }

    }catch(e){
          emailext  attachLog: true, subject: "${JOB_NAME}-Jenkins built number # ${BUILD_NUMBER} is failed", to: 'hanmantmtelange@gmail.com,hmt4275@gmail.com'
      throw e;
    }
}