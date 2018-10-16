#!/usr/bin/env groovy

node {
	 
	stage('checkout'){
		checkout scm
	}

	def dockerImage
    stage('build docker') {
        dockerImage = docker.build('pdy/tutos', '.')
    }

	stage('stop docker'){
		sh "docker-compose stop"
	}

	stage('start docker'){			
		sh "docker-compose up -d "
	}
}