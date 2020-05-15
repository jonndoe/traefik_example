example deployment based on testdriven.io blog record:

Dockerizing Django with Postgres, Gunicorn, and Nginx
Want to learn how to build this?

Check out the post.
Want to use this project?
Development

Uses the default Django development server.

    Rename .env.dev-sample to .env.dev.

    Update the environment variables in the docker-compose.yml and .env.dev files.

    Build the images and run the containers:

    $ docker-compose up -d --build

    Test it out at http://localhost:8000. The "app" folder is mounted into the container and your code changes apply automatically.

Production

Uses gunicorn + nginx.

    Rename .env.prod-sample to .env.prod and .env.prod.db-sample to .env.prod.db. Update the environment variables.

    Build the images and run the containers:

    $ docker-compose -f docker-compose.prod.yml up -d --build

    Test it out at http://localhost:1337. No mounted folders. To apply changes, the image must be re-built.
    
    
Simple CI/CD pipeline implemented here as well.

Local development: Django(Dockerized) + Postgres(Dockerized)
we dont use any conda env or virtualenv here for local development, all set in docker.
sudo docker-compose up --build  >>>>: is for local development


sudo docker-compose -f docker-compose.prod.local.yml up --build >>>>>: is for testing production deploy 
on local machine.
sudo docker-compose -f docker-compose.prod.local.yml down -v    >>>>>: is for stopping cont and removing volumes

SOME USEFULL COMMANDS:

ssh -o StrictHostKeyCheking=no root@REMOTE_IP_ADDRESS whoami
if we have error: >>>> add '-o UserKnownHostsFile=/dev/null '  >>>>
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@REMOTE_IP_ADDRESS
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@REMOTE_IP_ADDRESS mkdir /app
scp  -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -r ./app ./nginx ./postgresql ./.env.prod.db ./.env.prod ./docker-compose.prod.local.yml root@55.69.251.150:/apptest

sudo docker login docker.pkg.github.com -u jonndoe -p YOUR_API_TOKEN_GITHUB
if we have error: >>>> sudo apt install gnupg2 pass


ON REMOTE HOST:(AS ROOT):

$ ssh-keygen -t rsa
$ cat ~/.ssh/id_rsa.pub
$ vi ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/id_rsa

Copy the contents of the private key:
$ cat ~/.ssh/id_rsa

Exit from the SSH session, and then set the key as an environment variable on your local machine:
export PRIVATE_KEY='-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA04up8hoqzS1+APIB0RhjXyObwHQnOzhAk5Bd7mhkSbPkyhP1
...
iWlX9HNavcydATJc1f0DpzF0u4zY8PY24RVoW8vk+bJANPp1o2IAkeajCaF3w9nf
q/SyqAWVmvwYuIhDiHDaV2A==
-----END RSA PRIVATE KEY-----'
 
Add the key to the ssh-agent:
$ ssh-add - <<< "${PRIVATE_KEY}"

To test, run:
$ ssh -o StrictHostKeyChecking=no root@<YOUR_INSTANCE_IP> whoami
root




VOLUMES:
Minimal example to test if volumes works on you host machine(taken from Stackoverflow):

tester:
    image: alpine
    volumes:
        - "./hostmount/:/var/lib/graphite/storage/whisper"
    command: "sh -c 'echo hello >> /var/lib/graphite/storage/whisper/blabla'"

This will create a directory hostmount on the host, and write a "blabla" file in it, containing "hello";

$ docker-compose up
Creating repro24508_tester_1
Attaching to repro24508_tester_1
repro24508_tester_1 exited with code 0

$ cat hostmount/blabla
hello











