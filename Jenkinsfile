pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('CheckoutCode') {
          steps {
            sh '''echo "Building the server code..."
mvn -version
mkdir -p target
touch "target/server.war"
'''
            stash(name: 'server', includes: '**/*.war')
          }
        }

        stage('BuildDevCode') {
          steps {
            sh '''echo "Building the client code..."
npm install --save react
mkdir -p dist
cat > dist/index.html <<EOF
hello! welcome to Jenkins pipeline for CICD
EOF
touch "dist/client.js"
'''
            stash(name: 'client', includes: '**/dist/*')
          }
        }

      }
    }

    stage('UnitTest') {
      parallel {
        stage('CheckOutUnitTestCode') {
          steps {
            sh '''echo "mvn test -Dbrowser=chrome"
'''
          }
        }

        stage('RunUnitTests') {
          steps {
            sh 'echo "mvn test -Dbrowser=firefox"'
          }
        }

        stage('PublishReport') {
          steps {
            sh 'echo "Report Published for Unit Test"'
          }
        }

      }
    }

    stage('Checkout Regression test scripts') {
      parallel {
        stage('Check Smoke Test') {
          steps {
            unstash 'server'
            unstash 'client'
            sh '''echo "deploy to server ..."

'''
            echo 'APP_DIR=C:\\usr\\local\\tomcat\\webapps rm -rf $APP_DIR/ROOT cp target/server.war $APP_DIR/server.war mkdir -p $APP_DIR/ROOT cp dist/* $APP_DIR/ROOT C:\\usr\\local\\tomcat\\webapps\\startup.sh'
            sh 'echo ":Check out code from SVN for Testing "'
          }
        }

        stage('Run Smoke Test') {
          steps {
            sh 'echo " Running smoke test code"'
            sh 'echo "Publish reports :=="'
          }
        }

        stage('PublishE2EReport') {
          steps {
            input(message: 'Is Testing Passed', ok: 'Go Ahead with Deployment', submitter: 'mjena@radial.com')
          }
        }

      }
    }

    stage('Deploy To TST') {
      steps {
        echo 'APP_DIR=C:\\usr\\local\\tomcat\\webapps rm -rf $APP_DIR/ROOT cp target/server.war $APP_DIR/server.war mkdir -p $APP_DIR/ROOT cp dist/* $APP_DIR/ROOT C:\\usr\\local\\tomcat\\webapps\\startup.sh'
        sh 'echo "Deployment to the TST Server is done"'
      }
    }

    stage('Checkout Regression Test Code') {
      parallel {
        stage('Checkout Regression Test Code') {
          steps {
            sh 'echo "Checkout code from SVN"'
          }
        }

        stage('Run Regression Test') {
          steps {
            sh 'echo "Run Regression Code on TST"'
          }
        }

        stage('') {
          steps {
            sh 'echo "Publish Report for E2E on TST"'
          }
        }

      }
    }

  }
}