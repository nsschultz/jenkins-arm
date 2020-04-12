pipeline 
{
    agent any
    environment
    {
        VERSION_NUMBER = '0.1.0'
        IMAGE_VERSION = "${GIT_BRANCH == "master" ? VERSION_NUMBER : VERSION_NUMBER+"-"+GIT_BRANCH}"
    }
    stages 
    {
        stage('build') { steps { script { sh("docker build -t nschultz/jenkins:${IMAGE_VERSION} .") } } }
        stage('push')
        { 
            when { branch 'master' }
            steps
            {
                DOCKER_HUB = credentials("dockerhub-creds")
                script 
                {
                    sh  """
                        #!/bin/bash
                        docker login -u ${DOCKER_HUB_USR} -p ${DOCKER_HUB_PW}
                        docker push nschultz/jenkins:${IMAGE_VERSION}
                        docker logout
                        """ 
                } 
            }
        }
    }
}