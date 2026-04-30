node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        deleteDir()
    }

    stage('Clone Repo'){
        git(
            branch: 'main',
            url: 'https://github.com/amir-1122/jenkins-test'
        )
    }

    stage('Deploy to EC2'){
        sh """
            mkdir -p ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}

            npm config set fetch-timeout 600000
            npm config set fetch-retries 5

            npm ci --no-audit --prefer-offline

            npm run build

            fuser -k 3000/tcp || true

            nohup npm run start > app.log 2>&1 &
        """
    }
}