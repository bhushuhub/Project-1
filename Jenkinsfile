pipeline{
    agent{
        node{
            label 'built-in'
            customWorkspace '/mnt/fe415'
        }
    }
    stages{
        stage ('cloning repo'){
            steps{
                sh "rm -rf /mnt/fe415/*"
                sh "git clone -b fe415 'https://github.com/bhushuhub/Project-1.git'"
            }
        }
        stage ('mkdir fe415 directory'){
            agent{
                node{
                    label 'DEV'
                }
            }
            steps{
                sh "mkdir -p /mnt/fe415"
                sh "chmod -R 777 /mnt/fe415"
            }
        }
        stage ('scp files to slave-2'){
            agent{
                node{
                    label 'built-in'
                }
            }
            steps{
                sh "chmod -R 777 /mnt/fe415/Project-1/index.html"
                sh "scp -i '/mnt/new-keypair.pem' /mnt/fe415/Project-1/index.html ec2-user@172.31.87.198:/mnt/fe415/"
            }
        }
        
        stage ('on slave-2'){
            agent{
                node{
                    label 'DEV'
                    customWorkspace '/mnt/fe415'
                }
            }
            steps{
                sh "sudo yum install httpd -y"
                sh "sudo service httpd start"
                sh "sudo cp -r index.html /var/www/html/"
                sh "sudo chmod -R 777 /var/www/html/index.html"
            }
        }
    }
}
