node('master') {

stage('Checkout') {
	git 'https://github.com/arjunkundur/game-of-life.git'
}

stage('Build') {
    withMaven {
    sh "mvn install -DskipTests"
}
}

stage('Nexus_Deploy') {
	nexusArtifactUploader artifacts: [[artifactId: 'dev', classifier: '', file: 'gameoflife-web/target/gameoflife.war', type: 'war']], credentialsId: '44e5c24c-2ec4-4ac4-a599-58183becda76', groupId: 'dev', nexusUrl: '34.209.226.14:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'red', version: '$BUILD_ID'	
}

stage('Ansible_Playbook') {
	ansiblePlaybook extras: '-e BUILD_ID=${BUILD_ID}', inventory: '/home/ubuntu/inventory', playbook: 'playbook.yml'
	
}
stage('Application Status'){
	sh 'echo http://54.184.72.230:8080/dev-$BUILD_ID/'
	sh 'echo Click Above Link To Launch The Application'

}
	
}

