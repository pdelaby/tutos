#!/usr/bin/env groovy

node {
	 
	def commitId
	stage('checkout'){
		// attention à checkout avec les submodules : 
		// checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: true, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/pdelaby/tutos']]])
		checkout scm
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