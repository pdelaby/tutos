#!/usr/bin/env groovy

node {
	 
	def commitId
	stage('checkout'){
		cleanWs()
		checkout scm

		// checkout les submodules (le thème)
		sh 'git submodule update --init'
		commitId = sh(returnStdout: true, script: 'git rev-parse HEAD')

		commitId = commitId.trim()
	}

    stage('build docker') {
        docker.build("pdy/tutos:${commitId}", '.')
		sh "docker tag pdy/tutos:${commitId} pdy/tutos:latest"
    }

	stage('stop docker'){
		sh "docker-compose stop"
	}

	stage('start docker'){			
		sh "docker-compose up -d "
	}
}