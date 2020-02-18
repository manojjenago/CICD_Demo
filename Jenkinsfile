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

    stage('Run E2E Regression') {
      parallel {
        stage('Run E2E Regression') {
          steps {
            unstash 'server'
            unstash 'client'
            sh '''echo "deploy to server ..."

'''
            input(message: 'is QA Passed??', ok: 'Go Ahead with Deployment', submitter: 'mjena')
            echo 'APP_DIR=C:\\usr\\local\\tomcat\\webapps rm -rf $APP_DIR/ROOT cp target/server.war $APP_DIR/server.war mkdir -p $APP_DIR/ROOT cp dist/* $APP_DIR/ROOT C:\\usr\\local\\tomcat\\webapps\\startup.sh'
          }
        }

        stage('') {
          steps {
            sh 'echo "Check Out From SVN"'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'APP_DIR=C:\\usr\\local\\tomcat\\webapps rm -rf $APP_DIR/ROOT cp target/server.war $APP_DIR/server.war mkdir -p $APP_DIR/ROOT cp dist/* $APP_DIR/ROOT C:\\usr\\local\\tomcat\\webapps\\startup.sh'
      }
    }

  }
}