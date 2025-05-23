pipeline { 
    agent any 
 
    environment { 
        NODE_ENV = 'development' 
    } 
 
    stages { 
        stage('Clone Repository') { 
            steps { 
                echo 'Cloning GitHub repository...' 
                // Jenkins will automatically checkout source 
            } 
        } 
 
        stage('Install Dependencies') { 
            steps { 
                echo 'Installing Node.js dependencies...' 
                bat 'npm install' 
            } 
        } 
 
        stage('Deploy and Keep Running') { 
            steps { 
                echo 'Starting Node.js app in background on port 3000...' 
 
                // Kill any existing app on port 3000 
                bat ''' 
                @echo off 
                for /f "tokens=5" %%a in ('netstat -aon ^| find ":3000" ^| find "LISTENING"') do ( 
                    echo Killing PID %%a 
                    taskkill /F /PID %%a 
                ) 
                exit /b 0 
                ''' 
 
                // Start app in background using "start" command 
                bat 'start "NodeApp" /B node index.js' 
 
                // Wait indefinitely for user input to manually stop pipeline 
                input message: "Node.js app is running at http://localhost:3000. Click PROCEED to stop the server and finish the pipeline." 
            } 
        } 
    } 
 
    triggers { 
        githubPush() // Trigger on GitHub push 
    } 
}


