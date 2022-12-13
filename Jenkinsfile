pipeline{
    agent{
        node{
            label 'built-in'
            customWorkspace '/mnt/fe500'
        }
    }
    stages{
        stage ('cloning repo'){
            steps{
                sh "rm -rf /mnt/fe500/*"
                sh "git clone -b fe500 'https://github.com/bhushuhub/Project-1.git'"
            }
        }
        stage ('mkdir fe500 directory'){
            agent{
                node{
                    label 'UAT'
                }
            }
            steps{
                sh "mkdir -p /mnt/fe500"
                sh "chmod -R 777 /mnt/fe500"
            }
        }
        stage ('scp files to slave-3'){
            agent{
                node{
                    label 'built-in'
                }
            }
            steps{
                sh "chmod -R 777 /mnt/fe500/Project-1/index.html"
                sh "scp -i '/mnt/new-keypair.pem' /mnt/fe500/Project-1/index.html ec2-user@172.31.85.106:/mnt/fe500/"
            }
        }
        
        stage ('on slave-3'){
            agent{
                node{
                    label 'UAT'
                    customWorkspace '/mnt/fe500'
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
