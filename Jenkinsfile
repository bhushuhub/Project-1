pipeline{
    agent{
        node{
            label 'built-in'
            customWorkspace '/mnt/fe250'
        }
    }
    stages{
        stage ('cloning repo'){
            steps{
                sh "rm -rf /mnt/fe250/*"
                sh "git clone -b fe250 'https://github.com/bhushuhub/Project-1.git'"
            }
        }
        stage ('scp files to slave-1'){
            steps{
                sh "chmod -R 777 /mnt/fe250/Project-1/index.html"
                sh "scp -i '/mnt/new-keypair.pem' /mnt/fe250/Project-1/index.html ec2-user@172.31.87.25:/mnt/fe250/"
            }
        }
        
        stage ('on slave-1'){
            agent{
                node{
                    label 'QA'
                    customWorkspace '/mnt/fe250'
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
