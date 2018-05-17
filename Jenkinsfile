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
        sh 'zip -r sample.war sample'
    }
    stage('prepare-dev') {
        call_ansible("ansible -m template -a 'src=inventories/template.inv.j2 dest=inventories/${version}' localhost -e env_type=${version}")
        call_ansible('ansible-playbook -i inventories/${version} playbooks/create-containers.yml')
    }
    stage('socle-dev') {
        parallel(
            'java': {
                call_ansible('ansible-playbook -i inventories/${version} playbooks/install-jdk.yml')
            },
            'tomcat': {
                call_ansible('ansible-playbook -i inventories/${version} playbooks/install-tomcat.yml')
            }
        )
    }
    stage('configure') {
        call_ansible('ansible-playbook -i inventories/${version} playbooks/tomcat-service.yml')
    }
/*
    } catch (Exception e) {
        slackSend (color: '#FF0000', message: "Failed Job '${env.JOB_NAME}' ${env.BUILD_URL}")
        error(e.toString())
    }
*/
}
