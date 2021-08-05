node {

	step([$class: 'WsCleanup'])

  stage('Checkout Code') {    
  	checkout scm    
  }
          
  stage('Run Test') {
    docker.image('node:16-alpine3.11').inside {
			withEnv([
				/* Override the npm cache directory to avoid: EACCES: permission denied, mkdir '/.npm' */
				'npm_config_cache=npm-cache',
						/* Override HOME Path */
				'HOME=.',
				'PORT=3030'
			]) {
        sh 'npm install'
        sh 'npm test'
      }
    }
  }

  stage("Build frontend image"){    
      frontendImage = docker.build('andresbelo12/perficient-devops-ramp-up-ui:${BUILD_NUMBER}')   
  }

	stage("Pushing image to registry"){

		docker.withRegistry('', 'docker-hub') {
      frontendImage.push('latest')
    }
     
  }

  stage("Removing unnecessary images"){
    sh 'docker image prune -a -f --filter "label!=andresbelo12/perficient-devops-ramp-up-ui"'
  }


}