#!/usr/bin/env groovy

pipeline {
    agent { label 'linux' }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: env.BRANCH_NAME == 'master' ? '10' : '2'))
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
