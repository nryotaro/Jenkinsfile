node {
    def credentialsId = 'cred'
    def sourcedir = 'src'
    stage ('checkout the source') {
        checkout([$class: 'MercurialSCM', 
                   source: 'https://bitbucket.org/foobar',
                   revision: 'default',
                   revisionType: 'BRANCH',
                   subdir: sourcedir,
                   clean: true,
                   credentialsId: credentialsId])
    }
    stage('build the jar') {
        dir(sourcedir) {
          withEnv(["PATH+MAVEN=${tool 'Maven3.3.9'}/bin"]) {
              sh 'mvn clean package'
          }
        }
    }
    def dockerdir = 'docker'
    stage('checkout the Dockerfile') {
        checkout([$class: 'MercurialSCM', 
                   source: 'https://bitbucket.org/fizzbaz',
                   revision: 'feature/foo',
                   revisionType: 'BRANCH',
                   subdir: dockerdir,
                   clean: true,
                   credentialsId: 'cred'])
    }
    stage('build the docker image') {
        sh "cp ${sourcedir}/api/target/*.jar ${dockerdir}/api/"
        sh "${dockerdir}/dockerfiles/api/build.sh"
    }
    stage('push the docker image'){
        sh "${dockerdir}/dockerfiles/api/push.sh"
    }
} 