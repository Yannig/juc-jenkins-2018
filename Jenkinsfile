parameters = [
  booleanParam(name: 'simulate', description: "Mode simulation ?", defaultValue: false),
  booleanParam(name: 'refresh', description: "Rafraîchir paramètres du job ?", defaultValue: false),
]
// Retourne les properties à Jenkins pour savoir quoi demander à l'utilisateur
properties([ parameters(parameters) ])

// Sortie prématurée dans le cas où on veut juste rafraichir le job dans l'ancienne interface Jenkins
if("${env.refresh}" != "false") {
  echo "Rafraichissement simple des paramètres Jenkins."
  return
}

def call_ansible(cmd) {
    def env_value = [ "ANSIBLE_FORCE_COLOR=true", "PYTHONUNBUFFERED=1" ]
    withEnv(env_value) {
        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm']) {
            sh cmd
        }
    }
}

node {
    checkout scm
    stage('init') {
        // Récupération du nom de la branche (master, v1, v2 etc.)
        env.version     = env.BRANCH_NAME.split('/')[-1]
        env.destination = "/tmp/$version"
    }
    stage('build') {
        echo "Building sample.war"
        sh 'cd sample && zip -r ../sample.war *'
    }
/*    stage('prepare-dev') {
        call_ansible("ansible -m template -a 'src=inventories/template.inv.j2 dest=inventories/${version}' localhost -e env_type=${version}")
        call_ansible('ansible-playbook -i inventories/${version} playbooks/create-containers.yml')
    }*/
/*    stage('socle-dev') {
        parallel(
            'java': {
                call_ansible('ansible-playbook -i inventories/${version} playbooks/install-jdk.yml')
            },
            'tomcat': {
                call_ansible('ansible-playbook -i inventories/${version} playbooks/install-tomcat.yml')
            }
        )
    }*/
/*    stage('configure') {
        call_ansible('ansible-playbook -i inventories/${version} playbooks/tomcat-service.yml')
    }
    stage('deploy') {
        call_ansible('ansible-playbook -i inventories/${version} playbooks/deploy-application.yml -e war_to_deploy=$WORKSPACE/sample.war')
    }
    stage('clean-up') {
        userInput = input(
            id: 'Proceed1', message: 'Cleanup docker container?', parameters: [
                [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
            ]
        )

        call_ansible('ansible-playbook -i inventories/${version} playbooks/cleanup-containers.yml')
    }*/
/*
    } catch (Exception e) {
        slackSend (color: '#FF0000', message: "Failed Job '${env.JOB_NAME}' ${env.BUILD_URL}")
        error(e.toString())
    }
*/
}
