pipeline {
    agent any

    stages {
        stage('Package') {
            steps {
                sh 'mvn package' 
            }
        }
        stage('Analyse') {
            steps {
            	sh 'mvn checkstyle:checkstyle'
                sh 'mvn spotbugs:spotbugs'
                sh 'mvn pmd:pmd' 
            }
        }
        stage('Publish') {
            steps {
                archiveArtifacts '/target/*.jar'
            }
        }
		stage('Maven Publish') {
            steps {
				nexusPublisher nexusInstanceId: 'NexuxLocalJenkins', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [], mavenCoordinate: [artifactId: 'mon-appli', groupId: 'fr.ilias.hattane', packaging: 'jar', version: '0.1.0']]]
            }
        }
        
    }
    
    post {
        always {
            junit '**/surefire-reports/*.xml'
            
			recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
            recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
			
        }

    }

}