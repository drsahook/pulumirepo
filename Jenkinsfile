pipeline {
    agent any

    stages {
        stage ("Checkout code") {
            steps {
                git url: "git@github.com:drsahook/pulumirepo.git",
                    // Set your credentials id value here.
                    // See https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials
                    credentialsId: "d606e6ae-a550-4baa-8efd-6686a0281966",
                    // You could define a new stage that specifically runs for, say, feature/* branches
                    // and run only "pulumi preview" for those.
                    branch: "main"
            }
        }

        stage ("Install dependencies") {
            steps {
                sh "curl -fsSL https://get.pulumi.com | sh"
                sh "$HOME/.pulumi/bin/pulumi version"
            }
        }

        stage ("Pulumi up") {
            steps {
                // The value "node 8.9.4" is the configuration name in our Global Tool Configuration setup for node.
                // You should use the name that you used when you added the installation on that page.
                nodejs(nodeJSInstallationName: "node 8.9.4") {
                    withEnv(["PATH+PULUMI=$HOME/.pulumi/bin"]) {
                        sh "cd infrastructure && npm install"
                        sh "pulumi stack select ${PULUMI_STACK} --cwd infrastructure/"
                        sh "pulumi up --yes --cwd infrastructure/"
                    }
                }
            }
        }
    }
}