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
		sh "docker-compose -f gohugo.yml stop"
		sh "docker-compose -f gohugo.yml up -d "
	}
}