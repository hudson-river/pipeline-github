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
        stage('Test') {
            steps {
                script {
                    printf env
                    if (env.CHANGE_ID) {
                        if (pullRequest.comments.size()) {
                            def comment = pullRequest.comments.last()
                            if (comment.body =~ /^\/hudson .*/) {
                                // This is a command, mark the command as handled, and handle it
                                if (comment.body == "/hudson ping") {
                                    pullRequest.comment("PONG")
                                } else if (comment.body == "/hudson prenv") {
                                    pullRequest.comment("Starting a new pull request environment")
                                } else if (comment.body == "/hudson help" || comment.body == "/hudson ?") {
                                    pullRequest.comment("""Available commands:

* `/hudson help` -- this help
* `/hudson ping` -- pong
* `/hudson prenv` -- start a new prenv""")
                                }
                            }
                        } else {
                            pullRequest.comment("""<details>
<summary>Hudson commands and options</summary>
<br />

* `/hudson help` -- this help
* `/hudson ping` -- pong
* `/hudson prenv` -- start a new prenv
</details>
""")
                        }
                    } else {
                        echo "NOT a PR"
                    }

                }
            }
        }
    }
}

