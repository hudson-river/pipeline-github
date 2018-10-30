#!/usr/bin/env groovy

pipeline {
    agent { label 'linux' }

    triggers {
        issueCommentTrigger('^/hudson .*')
    }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: env.BRANCH_NAME == 'master' ? '10' : '2'))
    }

    stages {
        stage('Hudson') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        def triggerCause = currentBuild.rawBuild.getCause(org.jenkinsci.plugins.pipeline.github.trigger.IssueCommentCause)

                        if (triggerCause) {
                            def comment = triggerCause.comment
                            if (comment.body =~ /^\/hudson .*/) {
                                // This is a command, mark the command as handled, and handle it
                                if (comment.body == "/hudson ping") {
                                    pullRequest.comment("PONG")
                                } else if (comment.body == "/hudson prenv") {
                                    pullRequest.comment("Starting a new pull request environment for branch ${env.CHANGE_BRANCH}")
                                } else if (comment.body == "/hudson help" || comment.body == "/hudson ?") {
                                    pullRequest.comment("""Available commands:

* `/hudson help` -- this help
* `/hudson ping` -- pong
* `/hudson prenv` -- start a new prenv""")
                                }
                            }
                        } else {
                            if (pullRequest.comments.size() == 0) {
                                pullRequest.comment("""<details>
<summary>Hudson commands and options</summary>
<br />

* `/hudson help` -- this help
* `/hudson ping` -- pong
* `/hudson prenv` -- start a new prenv
</details>
""")
                            }
                        }
                    } else {
                        echo "NOT a PR"
                    }

                }
            }
        }

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

