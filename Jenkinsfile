#!groovy

@Library('SovrinHelpers') _

def name = 'sovrin-genesis'

def buildDebUbuntu = { repoName, releaseVersion, sourcePath ->
    def volumeName = "${name}-deb-u1604"
    if (env.BRANCH_NAME != '' && env.BRANCH_NAME != 'master') {
        volumeName = "${volumeName}.${BRANCH_NAME}"
    }
    if (sh(script: "docker volume ls -q | grep -q '^$volumeName\$'", returnStatus: true) == 0) {
        sh "docker volume rm $volumeName"
    }
    dir('build-scripts/ubuntu-1604') {
        sh "./build-${name}-docker.sh \"$sourcePath\" $releaseVersion $volumeName"
    }
    return "$volumeName"
}

env.SOVRIN_CORE_REPO_NAME = 'test' //FIXME rm test line

options = new TestAndPublishOptions()
options.enable([StagesEnum.PACK_RELEASE_COPY, StagesEnum.PACK_RELEASE_COPY_ST])
options.skip([StagesEnum.PYPI_RELEASE])
options.skip([StagesEnum.BUILD_RESULT_NOTIF, StagesEnum.QA_NOTIF, StagesEnum.PRODUCT_NOTIF, StagesEnum.TGB_NOTIF, StagesEnum.GITHUB_RELEASE]) //FIXME rm test line
options.setCopyWithDeps(false)
testAndPublish(name, [ubuntu: [:]], true, options, [ubuntu: buildDebUbuntu])
