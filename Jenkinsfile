pipeline{
        agent none
        stages{
            stage('BUILD'){
                        agent {
                              label 'java-slave-2'
                              }
                    steps{
                         sh '''
                             #!/bin/bash
                             echo " this is a web based maven build project"
                             echo " this is built on java-slave-2 server"
                             cd /opt/jenkins/workspace/java-web-mvn-pipeline/
                             mvn install
                             if [ $? -eq 0 ] ; then
                             echo "built successfully"
                             fi
                             sudo chmod 777 /opt/jenkins/workspace/java-web-mvn-pipeline/target/*.war
                             cd ~
                             sudo scp -i Dec-devops-2021.pem /opt/jenkins/workspace/java-web-mvn-pipeline/target/*.war  ec2-user@172.31.5.125:/usr/share/tomcat/webapps/
                             '''
                        }
                       }
                stage('TEST'){
                         agent {
                                label 'tomcat-server'
                              } 
                        steps{
                         sh '''
                              #!/bin/bash
                              echo " this is to test the .war file in tomcat server"
                              sudo service tomcat start
                              cd ~
                              sudo scp -i Dec-devops-2021.pem /usr/share/tomcat/webapps/*.war ec2-user@172.31.23.172:/home/ec2-user/
                            '''
                          }
                       }
                stage('DEPLOY'){
                         agent {
                                label 'java-s1'
                                }
                        steps{
                          sh 'echo "deployed to java-s1 successfully"'
                            }
                          }
        }
}
