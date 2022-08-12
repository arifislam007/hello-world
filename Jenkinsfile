pipeline {
    agent any
    tools {
      maven 'maven'
    }
    stages {
        stage('SCM') { 
            steps {
                git 'https://github.com/arifislam007/hello-world.git' 
            }
        }
		stage('Maven buil') { 
            steps {
               sh "mvn clean install"
            }
        }
		stage('Send Docker File') { 
            steps {
               sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', 
			   transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', 
			   execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, 
			   patternSeparator: '[, ]+', remoteDirectory: 'dockertomcatv2', remoteDirectorySDF: false, 
			   removePrefix: '', sourceFiles: 'Dockerfile')], usePromotionTimestamp: false, 
			   useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage('Send Over SSH Docker Host') {
			steps {
				sshPublisher(publishers: [sshPublisherDesc(configName: 'dockerhost', 
					transfers: [sshTransfer(cleanRemote: false, excludes: '', 
					execCommand: '''cd /root/dockertomcatv2;
					docker build -t tomapp:v5 .;
					docker stop tomapp5;
					docker rm tomapp5;
					docker run -itd --name tomapp5 -p 8024:8080 tomapp:v5''', 
					execTimeout: 120000, flatten: false, makeEmptyDirs: false, 
					noDefaultExcludes: false, patternSeparator: '[, ]+', 
					remoteDirectory: 'dockertomcatv2', remoteDirectorySDF: false, 
					removePrefix: 'webapp/target', sourceFiles: '**/*.war')], 
					usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
               
            }
            
        }
    }
}