#!/usr/bin/env groovy

node {
	 
	stage('checkout'){
		checkout scm
	}

    stage('build docker') {
        docker.build('pdy/tutos', '.')
    }

	stage('stop docker'){
		sh "docker-compose stop"
	}

	stage('start docker'){			
		sh "docker-compose up -d "
	}
}