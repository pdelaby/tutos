#!/usr/bin/env groovy


node {
	 
	stage('checkout'){
		checkout scm
	}

	def dockerImage
    stage('build docker') {
        dockerImage = docker.build('pdy-doc', '.')
    }
		
	stage('start docker'){			
		sh "docker-compose stop"
		sh "docker-compose up -d "
	}
}