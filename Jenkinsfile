pipeline{
    agent any
    stages{
        stage('Making Grid UP'){
            steps{
                bat "docker-compose -f grid.yaml up -d"
            }
        }
        stage('Running Tests'){
            steps{
                bat "docker-compose -f test_suite.yaml up"
                script{
                    if(fileExists("output/test_results1/testng-failed.xml") || fileExists("output/test_results1/testng-failed.xml")){
                        error("Failed test case found..So failing Entire build..")
                    }
                }
            }
        }
    }
    post{
        always{
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test_suite.yaml down"
    }
   
}