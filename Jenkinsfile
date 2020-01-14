pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Server') {
          steps {
            sh '''echo "Building the server code..."
mvn -version
mkdir -p target
touch "target/server.war"
'''
            stash(name: 'server', includes: '**/*.war')
          }
        }

        stage('Client') {
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

  }
}