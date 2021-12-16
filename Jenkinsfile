pipeline {
    agent any

    stages {
        stage('Package') {
            steps {
            	bat 'mvn clean'
                bat 'mvn package' 
            }
        }
        stage('Analyse') {
            steps {
            	bat 'mvn checkstyle:checkstyle'
                bat 'mvn spotbugs:spotbugs'
                bat 'mvn pmd:pmd' 
            }
        }
        stage('Publish') {
            steps {
                archiveArtifacts '/target/*.jar'
            }
        }
		stage('Push into Maven nexus') {
            steps {
nexusPublisher nexusInstanceId: 'NexuxLocalJenkins', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/mon-appli-0.2.0.jar']], mavenCoordinate: [artifactId: 'mon-appli', groupId: 'fr.ilias.hattane', packaging: 'jar', version: '0.2.0']]]            }
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