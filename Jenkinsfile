#!/usr/bin/env groovy

pipeline {
    agent { label 'linux' }

    checkout([
        $class: 'GitSCM',
        branches: scm.branches,
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CloneOption', depth: 0, noTags: false, reference: '', shallow: true]], submoduleCfg: [], userRemoteConfigs: [[]]
        ])

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: env.BRANCH_NAME == 'master' ? '11' : '2'))
    }

    stages {
        stage('Test') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        pullRequest.comment("""Some tests failed

<details><summary>parsePropsToComponent.SellersPageWithData parsePropsToComponent</summary>
<br />

```
Error: expect(value).toMatchSnapshot()
```
</details>""")
                    }
                }
            }
        }

    }
}
