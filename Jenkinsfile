pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
               sh 'pip3 install -r requirements.txt'
            }
        }
        stage('Test') { 
            steps {
                sh 'python manage.py test' 
            }
        }
        stage('Deploy to staging area') { 
            input{
                massage "shall we deploy to the production?"
                ok 'yes please'
            }
            steps {
                sh 'ssh -o StricHostKeyChecking=no deployment-user@192.168.56.107 "source venv/bin/activate; \ 
                cd polling;\
                git pull origin master;\
                pip install -r requirements.txt --no-warn-script-location;\
                python manage.py migrate;\
                deactivate;\
                sudo systemctl restart nginx;\
                sudo systemctl restart gunicorn"'
            }

        stage('Deploy to production') { 
            steps {
                sh 'ssh -o StricHostKeyChecking=no deployment-user@192.168.56.101 "source venv/bin/activate; \ 
                cd polling;\
                git pull origin master;\
                pip install -r requirements.txt --no-warn-script-location;\
                python manage.py migrate;\
                deactivate;\
                sudo systemctl restart nginx;\
                sudo systemctl restart gunicorn"'
            }
        }
    }
}