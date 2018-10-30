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
                        if (pullRequest.comments.size()) {
                            def comment = pullRequest.comments.last()
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

        stage('Test') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        pullRequest.comment("""Some tests failed

<details><summary>Run parallel RSpec tests / JS-tests / SellersPageWithData parsePropsToComponent.SellersPageWithData parsePropsToComponent</summary>
<br />

```
Error: expect(value).toMatchSnapshot()

Received value does not match stored snapshot "SellersPageWithData parsePropsToComponent 1".

- Snapshot
+ Received

  Object {
    "error": null,
-   "listing": Object {},
    "loading": true,
+   "sellersPage": undefined,
  }
    at Object.<anonymous> (/hemnet-ng/app/assets/components/sellers-page/main/SellersPageWithData.test.js:11:23)
    at Object.asyncJestTest (/hemnet-ng/node_modules/jest-jasmine2/build/jasmine_async.js:108:37)
    at resolve (/hemnet-ng/node_modules/jest-jasmine2/build/queue_runner.js:56:12)
    at new Promise (<anonymous>)
    at mapper (/hemnet-ng/node_modules/jest-jasmine2/build/queue_runner.js:43:19)
    at promise.then (/hemnet-ng/node_modules/jest-jasmine2/build/queue_runner.js:87:41)
    at <anonymous>
```
</details>

<details><summary>Run parallel RSpec tests / split-1 / spec.graph.consumer_api.query_root_spec.GraphQL type ConsumerAPI::QueryRoot has tests for all fields</summary>

```
Failure/Error:
  expect(uncovered_names).to(
    be_empty, "Missing tests for these members:\n * #{uncovered_names.join("\n * ")}"
  )

  Missing tests for these members:
   * sellersPage
./spec/support/graph_spec_helpers.rb:97:in `block in expect_covered'
./spec/support/graph_spec_helpers.rb:95:in `instance_eval'
./spec/support/graph_spec_helpers.rb:95:in `expect_covered'
./spec/support/graph_spec_helpers.rb:22:in `block (2 levels) in describe_type'
./spec/spec_helper.rb:75:in `block (2 levels) in <top (required)>'
./spec/spec_helper_light.rb:110:in `block (3 levels) in <top (required)>'
/usr/local/bundle/gems/graphql-batch-0.3.10/lib/graphql/batch.rb:15:in `batch'
./spec/spec_helper_light.rb:109:in `block (2 levels) in <top (required)>'
```
</details>""")
                    }
                }
            }
        }
    }
}

