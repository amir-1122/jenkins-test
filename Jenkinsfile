node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        deleteDir()
    }

    stage('Clone Repo'){
        git(
            branch: 'main',
            url: 'https://github.com/farzeen-ali/CICD-Jenkins-AWS'
        )
    }

    stage('Deploy to EC2'){
        sh """
            mkdir -p ${appDir}
            chown -R jenkins:jenkins ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}

            # Fix npm network issues
            npm config set fetch-timeout 600000
            npm config set fetch-retries 5
            npm config set registry https://registry.npmjs.org/

            # Clean install (better for CI)
            npm ci --no-audit --prefer-offline

            # Build app
            npm run build

            # Kill old process
            fuser -k 3000/tcp || true

            # Start in background (IMPORTANT)
            nohup npm run start > app.log 2>&1 &
        """
    }
}