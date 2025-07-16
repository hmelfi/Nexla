pipeline {
    agent any

    parameters {
        text(name: 'nexla_CLI_flow_import_inputs', defaultValue: '', description: 'Paste your Nexla CLI flows import input text here')
    }

    stages {
        stage('Download Nexla Dev Export File from GitHub') {
            steps {
                sh '''
                    echo "Downloading file from GitHub..."
                    curl -sSL -o flow481798.json https://raw.githubusercontent.com/hmelfi/Nexla/main/flow481798.json
                    echo "Download complete. File saved as flow481798.json"
                '''
            }
        }

        stage('Process Input') {
            steps {
                script {
                    // Save the parameter input to a file
                    writeFile file: 'input.txt', text: params.nexla_CLI_flow_import_inputs
                }
                echo "CLI will run using input.txt"
            }
        }

        stage('Run CLI') {
            steps {
                sh '''
                  #!/bin/bash
                  echo "Running CLI with input:"
                  cat input.txt

                  # Example CLI call
                  nexla flows import -i flow481798.json < input.txt
                '''
            }
        }
    }
}
