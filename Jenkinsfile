pipeline {
    agent any

    parameters {
        text(name: 'nexla_CLI_flow_import_inputs', defaultValue: '', description: 'Paste your Nexla CLI flows import input text here')
	string(name: 'dev_flow_export_spec_filename', defaultValue: '', description: 'Name of the Nexla export spec file to fetch from GitHub')
    }

    stages {
        stage('Download Nexla Dev Export File from GitHub') {
            steps {
		script {
                    def url = "https://raw.githubusercontent.com/hmelfi/Nexla/main/${params.dev_flow_export_spec_filename}"
                    echo "Downloading file from GitHub: ${params.dev_flow_export_spec_filename}"
                    sh """
                        curl -sSL -o ${params.dev_flow_export_spec_filename} ${url}
                        echo "Download complete. File saved as ${params.dev_flow_export_spec_filename}"
                    """
                }
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
                sh """
                  #!/bin/bash
                  echo "Running CLI with input file:"
                  cat input.txt
                  echo "Running CLI export file: ${params.dev_flow_export_spec_filename}"

                  # Example CLI call
                  /Library/Frameworks/Python.framework/Versions/3.13/bin/nexla flows import -i ${params.dev_flow_export_spec_filename} < input.txt
                """
            }
        }
    }
}
