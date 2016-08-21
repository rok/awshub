# awshub
An EC2 based deployment of jupyterhub (based on https://github.com/jupyterhub/jupyterhub-deploy-teaching)

Requirements: ansible, python 2

To spin-up a jupyterhub node in EC2 run:
- git clone git@github.com:rok/awshub.git && cd awshub
- git clone git@github.com:UDST/ansible-conda.git ./ansible-conda
- (Optional) Edit roles/cloudflare/defaults/main.yml to add cloudflare credentials for your domain.
- Run ansible-playbook ./site.yml
- Jupyterhub should be running on https://yourdomain.com if you've configured it or on a random EC2 IP.
