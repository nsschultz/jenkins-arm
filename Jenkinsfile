pipeline 
{
    agent { label 'builder' }
    environment
    {
        VERSION_NUMBER = '0.3.4'
        IMAGE_VERSION = "${GIT_BRANCH == "master" ? VERSION_NUMBER : VERSION_NUMBER+"-"+GIT_BRANCH}"
        DOCKER_HUB = credentials("dockerhub-creds")
    }
    stages 
    {
        stage('build') { steps { script { sh("docker build -t nschultz/jenkins:${IMAGE_VERSION} .") } } }
        stage('push')
        { 
            when { branch 'master' }
            steps
            {
                script 
                {
                    sh  """
                        #!/bin/bash
                        docker login -u ${DOCKER_HUB_USR} -p ${DOCKER_HUB_PSW}
                        docker push nschultz/jenkins:${IMAGE_VERSION}
                        docker logout
                        """ 
                } 
            }
        }
    }
}