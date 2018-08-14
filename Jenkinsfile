def integrationTests = [:]
def cciRhelTags = []
def iso_build = 'null'
def clean_unix() {
    sh '''
    rm -fr /tmp/minishift*
    rm -fr /tmp/openshift*
    virsh connect qemu:///system || echo "error connecting to qemu:///system"
    virsh destroy minishift || echo "no domain minishift"
    '''
}
def debug_unix() {
    sh '''
    rpm -qa || echo "failed"
    whoami || echo "failed"
    ifconfig || echo "failed"
    '''
}
def prepare_unix() {
    sh '''
    mkdir -p go/src/github.com/minishift go/bin go/pkg minishift
    sudo yum install golang -y
    go get -u github.com/golang/dep/cmd/dep

    curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.7.0/docker-machine-driver-kvm -o minishift/docker-machine-driver-kvm
    chmod 755 minishift/docker-machine-driver-kvm
    curl -k http://10.70.48.26/builds/linux-amd64/minishift -o minishift/minishift
    chmod 755 minishift/minishift

    git clone --depth 0 --single-branch -b $BRANCH https://$MINISHIFT_GITHUB_API_TOKEN@$FORK go/src/github.com/minishift/minishift
    '''
}

// QUICK TESTS
if (env.QUICK_TEST == "true") {
    cciRhelTags=[
        ['cmd-version','setup-cdk','basic']]
    integrationTests["blr-rhel7-smoke"] = {
        stage('blr-rhel7-smoke'){[
            retry(2){ build job: "tests-rhel7/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            catchError{retry(2){ build("tests-rhel7/basic.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-version.feature")}}]
        }
    }
    integrationTests["blr-win7-smoke"] = {
        stage('blr-win7-smoke'){[
            retry(2){ build job: "tests-win7/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            catchError{retry(2){ build("tests-win7/basic.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-version.feature")}}]
        }
    }
    integrationTests["blr-mac10-smoke"] = {
        stage('blr-mac10-smoke'){[
            retry(2){ build job: "tests-mac10/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            catchError{retry(2){ build("tests-mac10/basic.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-version.feature")}}]
        }
    }
// FULL TESTS
} else {
    cciRhelTags = [
        ['basic','cmd-oc-env','cmd-docker-env'],
        ['cmd-addons','cmd-openshift','experimental-flags'],
        ['flags','provision-various-versions'],
        ['proxy','cmd-config','cmd-version','setup-cdk']]
    integrationTests["blr-rhel7-smoke"] = {
        stage('blr-rhel7-smoke'){[
            retry(2){ build job: "tests-rhel7/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            //catchError{retry(2){ build("tests-rhel7/addon-xpaas.feature")}},
            catchError{retry(2){ build("tests-rhel7/basic.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-addons.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-config.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-docker-env.feature")}},
            //catchError{retry(2){ build("tests-rhel7/cmd-image.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-oc-env.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-openshift.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-profile.feature")}},
            catchError{retry(2){ build("tests-rhel7/cmd-version.feature")}},
            catchError{retry(2){ build("tests-rhel7/flags.feature")}},
            catchError{retry(2){ build("tests-rhel7/provision-various-versions.feature")}},
            catchError{retry(2){ build("tests-rhel7/proxy.feature")}}]
        }
    }
    integrationTests["blr-win7-smoke"] = {
        stage('blr-win7-smoke'){[
            retry(2){ build job: "tests-win7/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            //catchError{retry(2){ build("tests-win7/addon-xpaas.feature")}},
            catchError{retry(2){ build("tests-win7/basic.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-addons.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-config.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-docker-env.feature")}},
            //catchError{retry(2){ build("tests-win7/cmd-image.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-oc-env.feature")}},
            //catchError{retry(2){ build("tests-win7/cmd-openshift.feature")},
            catchError{retry(2){ build("tests-win7/cmd-profile.feature")}},
            catchError{retry(2){ build("tests-win7/cmd-version.feature")}},
            catchError{retry(2){ build("tests-win7/flags.feature")}},
            catchError{retry(2){ build("tests-win7/provision-various-versions.feature")}},
            catchError{retry(2){ build("tests-win7/proxy.feature")}}]
        }
    }
    integrationTests["blr-mac10-smoke"] = {
        stage('blr-mac10-smoke'){[
            retry(2){ build job: "tests-mac10/_prepare", parameters:[
                string(name: 'FORK', value: "${FORK}"),
                string(name: 'BRANCH', value: "${BRANCH}")
            ]},
            //catchError{retry(2){ build("tests-mac10/addon-xpaas.feature")}},
            //catchError{retry(2){ build("tests-mac10/basic.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-addons.feature")}},
            //catchError{retry(2){ build("tests-mac10/cmd-config.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-docker-env.feature")}},
            //catchError{retry(2){ build("tests-mac10/cmd-image.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-oc-env.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-openshift.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-profile.feature")}},
            catchError{retry(2){ build("tests-mac10/cmd-version.feature")}},
            catchError{retry(2){ build("tests-mac10/flags.feature")}},
            catchError{retry(2){ build("tests-mac10/provision-various-versions.feature")}},
            catchError{retry(2){ build("tests-mac10/proxy.feature")}}]
        }
    }
}

// PARALLEL CCI JOBS
for (int i = 0; i < cciRhelTags.size(); i++) {
    def a = i //to pass copy, not link

    integrationTests["cci-rhel7-part${a}"] = {
        node('rhel7-16gb') {
            stage("cci-rhel7-part-${a}") {
                deleteDir()
                withCredentials([
                    usernamePassword(credentialsId: 'agajdosi-rhd-login', usernameVariable: 'MINISHIFT_USERNAME', passwordVariable: 'MINISHIFT_PASSWORD'),
                    usernamePassword(credentialsId: '5fc8e763-ddc7-4d66-a59f-f60819d99e9b', usernameVariable: 'GITLAB_USERNAME', passwordVariable: 'GITLAB_PASSWORD'),
                    string(credentialsId: 'agajdosi-gh-token', variable: 'MINISHIFT_GITHUB_API_TOKEN')
                ]) {
                    withEnv([
                        "PATH+GO=${WORKSPACE}/go/bin",
                        "PATH+MINISHIFT=${WORKSPACE}/minishift",
                        "GOPATH=${WORKSPACE}/go",
                        "FORK=${FORK}",
                        "BRANCH=${BRANCH}"
                    ]){
                        debug_unix()
                        clean_unix()
                        prepare_unix()
                        script {
                            for (int x = 0; x < cciRhelTags[a].size(); x++) {
                                catchError {
                                    retry(2) {
                                        sh "make --directory=\$GOPATH/src/github.com/minishift/minishift integration MINISHIFT_BINARY=\$(pwd)/minishift/minishift GODOG_OPTS=-tags=${cciRhelTags[a][x]}"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

// PIPELINE DEFINITION
pipeline {
    agent none
    parameters {
        choice(choices: 'nightly\nweekly', description: 'Nightly by default, weekly sometimes.', name: 'BUILD_TYPE')
        string(defaultValue: 'NONE', description: 'Name of the release (alpha2, beta0 .. etc), empty for nightly.', name: 'RELEASE_NAME')
        string(defaultValue: 'github.com/redhat-developer/minishift.git', description: 'Fork from which build', name: 'FORK')
        string(defaultValue: 'master', description: 'Release branch', name: 'BRANCH')
        string(defaultValue: 'master', description: 'Branch/Tag from where ISO should be build.', name: 'ISO_BRANCH')
        booleanParam(defaultValue: false, description: "To speed up things a little bit when it's needed...", name: 'QUICK_TEST')
        booleanParam(defaultValue: false, description: "Will skip the ISO and CDK builds and only run tests and binary upload.", name: 'SKIP_BUILD')
    }
    triggers {
        cron("H 18 * * 1-5")
    }
    options {
        timestamps()
        disableConcurrentBuilds()
    }
    stages {

        stage('BUILD ISO') {
            steps {
                script {
                    if (env.SKIP_BUILD == "false") {
                        iso_build = build(job: "cdk_iso", parameters: [
                            string(name: 'BRANCH', value: "${ISO_BRANCH}"),
                            string(name: 'PR_NUMBER', value: 'NONE')
                        ])
                    } else {
                        echo "ISO build skipped."
                    }
                }
            }
        }
    
        stage('BUILD CDK') {
            steps {
                script {
                    if (env.SKIP_BUILD == "false") {
                        build job: "cdk_build", parameters: [
                            string(name: 'ISO_URL', value: iso_build.buildVariables.iso_url),
                            string(name: 'FORK', value: "${FORK}"),
                            string(name: 'BRANCH', value: "${BRANCH}"),
                            string(name: 'PR_NUMBER', value: 'NONE')
                        ]
                    } else {
                        echo "CDK build skipped."
                    }
                }
            }
        }

        /*
        stage('upload artifacts') {
            steps {
                echo "TODO: job which uploads binaries to temporary location on artifact server"
            }
        }
        */

        stage('INTEGRATION TESTS'){
            steps {
                script {
                    parallel integrationTests
                }
            }
        }

        stage('PUBLISH ARTIFACTS') {
            steps {
                build job: "cdk_upload_artifact", parameters: [
                    string(name: 'buildType', value: "${BUILD_TYPE}"),
                    string(name: 'releaseNumber', value: "${RELEASE_NAME}")
                ]
            }
        }
    }
}
