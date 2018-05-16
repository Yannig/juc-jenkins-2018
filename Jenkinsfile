node {
    checkout scm
    stage('init') {
        // Récupération du nom de la branche (master, v1, v2 etc.)
        env.version     = env.BRANCH_NAME.split('/')[-1]
        env.destination = "/tmp/$version"
        sh('mkdir -p $destination.new')
        sh('git submodule update --init')
    }
    stage('build') {
        echo "Building sample.war"
        sh 'zip -r /tmp/$version/sample.war sample'
    }
    stage('publication') {
        sh '[ -d $destination ] && mv $destination $destination.old || exit 0'
        sh 'mv $destination.new $destination'
        sh 'rm -rf $destination.old'
    }
/*
    } catch (Exception e) {
        slackSend (color: '#FF0000', message: "Failed Job '${env.JOB_NAME}' ${env.BUILD_URL}")
        error(e.toString())
    }
*/
}
