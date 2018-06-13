pipeline {
    agent any

    stages {

        stage('Clone repository') {
            steps{
                checkout scm
            }
        }

        def app
        def buildType = env.BRANCH_NAME.split('/')[0]
        def dockerTag
        def pullRequest = false     //Boolean flag

        stage('Set variables'){
          steps {    // Can only do this after a the SCM has been pulled.
              env.GIT_TAG_NAME = gitTagName()
              def gitTagName = env.GIT_TAG_NAME
              env.GIT_TAG_MESSAGE = gitTagMessage()
              if (buildType in ['develop']) {
                  // develop branch will keeps develop as tag name
                  // Should we consider latest?
                  dockerTag = buildType
              } else if (buildType in ['master']) {
                  // Build master branch as tagname
                  // Maybe add an error if there is no tag on master branch - or just don't build.
                  // Or figure out how to get Jenkins to build and tag.
                  dockerTag = gitTagName
              } else if ( buildType ==~ /PR-./ ){
                  //   This is a pull request : do everything except push to repo
                  dockerTag = buildType
                  pullRequest = true
              } else if ( buildType in ['release'] ){
                  // BRANCH_NAME : 'release/X.Y.Z' or 'release/X.Y' or 'release/X'
                  //   This is a release - either major, feature, fix
                  //   Recomended to always use X.Y.Z to make sure we build properly
                  version = ( env.BRANCH_NAME.split('/')[1] )
                  dockerTag = "${version}-latest"
              } else {
                  dockerTag = ( env.BRANCH_NAME.split('/')[1] =~ /.+-\d+/ )[0]
              }
          }
        }
        stage('Print information'){
          steps {
            println("BRANCH_NAME : ${BRANCH_NAME}")
            println("GIT_TAG_NAME : ${GIT_TAG_NAME}")
            println("DockerTag : ${dockerTag}")
            // input("Branch ${env.BRANCH_NAME} -> ${buildType} : ${dockerTag} - OK to continue?")
          }
        }


        stage('Example') {
            steps {
                echo 'Building Docker image'
                sh './build'
            }
        }
    }
    post {
        always {
            echo 'Now What'
        }
    }
}
