def collectd=''
def filedata=''
def ssh=''
def rrdtool=''
def xml_definition=''
pipeline {
    agent none
    environment {
        COLLECTD_DIR="collectd"
        COLLECTD_REPO="https://github.com/yangx-jy/ScaleWX-collectd.git"
        XML_DEFINITION_DIR="xml_definition"
        XML_DEFINITION_REPO="https://github.com/yangx-jy/xml_definition.git"
        WORK_DIR="/var/lib/jenkins/work"
    }
    stages {
        stage('Build') {
            agent { label 'vm-rhel9.6' }
            steps {
                dir(COLLECTD_DIR) {
                    sh 'rm -rf *'
                    checkout([$class: 'GitSCM', branches: [[name: '*/test']], extensions: [], userRemoteConfigs: [[url: COLLECTD_REPO]]])
                    sh './cleanup.sh'
                    sh './build.sh && ./configure && make rpms'
                }
                dir(XML_DEFINITION_DIR) {
                    sh 'rm -rf *'
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: XML_DEFINITION_REPO]]])
                    sh './bootstrap.sh && ./configure && make rpm'
                }
            }
        }
    }
}
