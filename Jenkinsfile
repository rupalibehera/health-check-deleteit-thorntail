@Library('github.com/fabric8io/osio-pipeline@master') _

osio {

  config runtime: 'java', version: '1.8.1'

  ci {
    echo "Running CI build......"


    def resources = processTemplate(params: [
          release_version: "1.0.${env.BUILD_NUMBER}"
    ])
    integrationTestCmd =
         "mvn org.apache.maven.plugins:maven-failsafe-plugin:integration-test \
            org.apache.maven.plugins:maven-failsafe-plugin:verify \
            -Dnamespace.use.current=false -Dnamespace.use.existing=${utils.testNamespace()} \
            -Dit.test=*IT -DfailIfNoTests=false -DenableImageStreamDetection=true \
            -P openshift-it"
    build resources: resources, commands: integrationTestCmd
    

  }

  cd {
    echo "Running CD build......"

    def resources = processTemplate(params: [
          release_version: "1.0.${env.BUILD_NUMBER}"
    ])

    build resources: resources
    deploy resources: resources, env: 'stage'
    deploy resources: resources, env: 'run', approval: 'manual'

  }
}
