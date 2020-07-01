pipeline {
    agent any
    options {
        timeout(time: 60)
        }
    stages {
        stage("Path") {
            steps {
                echo("${PATH}")
            }
        }
        stage("Input") {
            steps {
                script {

                    // Variables for input
                    def inputUserid
                    def inputPass
                    // Get the input
                    def userInput = input(
                            id: 'userInput', message: 'Enter path of test reports:?',
                            parameters: [
                                    string(defaultValue: '', description: 'User-ID for MF', name: 'UserID'),
                                    password(defaultValue: '', description: 'Password', name: 'PassWd'),
                            ])

                    // Save to variables. Default to empty string if not found.
                    inputUser = userInput.UserID?:''
                    inputPass = userInput.PassWd?:''

                    // Echo to console
                    echo("Your user-id:  ${inputUser}")
                    echo("Your Password: ${inputPass}")
                    
                    sh 'zowe files list ds "MARTA02.*" --user ' | "${inputUser}" ' --password ' | "${inputPass}"                    
                    
                    // Write to file
                    writeFile file:  "inputData.txt", text: "Userid=${inputUser}\r\nPass=${inputPass}"
                    // Archive the file (or whatever you want to do with it)
                    archiveArtifacts 'inputData.txt'
                    
                }
            }
        }
        stage("Run CLI") {
            steps {
                sh 'echo("Run CLI stage")' 
                //sh 'zowe files list ds "MARTA02.*" --user "${inputUser}" --password "${inputPass}"'
            }
        }
    }
}
