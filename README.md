# coding-project-template

# build client
cd /home/project/xrwvm-fullstack_developer_capstone/server/frontend
npm install
npm run build

# start up server
cd /home/project/xrwvm-fullstack_developer_capstone/server
pip install virtualenv
virtualenv djangoenv
source djangoenv/bin/activate
python3 -m pip install -U -r requirements.txt
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver

# migrations for models
python3 manage.py makemigrations
python3 manage.py migrate --run-syncdb

# start up db
cd /home/project/xrwvm-fullstack_developer_capstone/server/database
docker build . -t nodeapp
docker-compose up

# start up sentianalyzer
cd xrwvm-fullstack_developer_capstone/server/djangoapp/microservices
docker build . -t us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
docker push us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
ibmcloud ce application create --name sentianalyzer --image us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer --registry-secret icr-secret --port 5000

# create deployment
kubectl apply -f deployment.yaml
kubectl port-forward deployment.apps/dealership 8000:8000

# if not working
check djangoapp/.env and djangoproj/settings.py for links