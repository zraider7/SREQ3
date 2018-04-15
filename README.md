# RundeckToAnsible

How the workflow works:
RunDeck (CreateUser or DeleteUser)-> Ansible to run yaml file-> Servers to push it to

A brief summary:
So everything is run through RunDeck, which just puts a UI to the Ansible calls. There are two jobs in there called CreateUser and DeleteUser.
These jobs are configured to accept both a username and lists a few servers to push to. Once those variables are filled out and the job runs, 
it calls out to ansible using "Ansible-playbook" and supplies the variables at the end. Both Ansible and rundeck are on the same server, so we don't
have to set up more nodes. Then reaches out to those servers chosen from rundeck and makes the new user or deletes them if they exist.

Ansible:
I chose this CM tool since I am familiar with it. I spun up three other centos nodes to be the clients. Once spun up, I created an ssh key and copied it 
over to those three nodes so I could communicate with them without a password. Crucial for Ansible. Then I set up the groups in the Ansible hosts file for
the servers. Then went to create the two playbooks for this project: CreateUser.yml and DeleteUser.yml, which is supplied in the repo. The CreateUser.yml accepts
a variable for the Username and the Host, so that we can name the user whatever we want and on any of the 3 nodes (or all three). Used the package "PassLib"
in order to make a hashed password to use in the CreateUser.yml script. The DeleteUser.yml works the same, for the most part. It goes out to the server and deletes
the user inputted, if they exist.

Rundeck:
I used this in order to make it simple for anyone to do the above operations with Ansible. I installed it on the same box with Ansible. Once set up, I created a project
called Linuxtopic and made the two jobs: CreateUser and DeleteUser. In the Job, instead of using the "Run Ansible Playbook" option, I have it set to run the 
"Ansible-Playbook" followed by the location of the playbook. This way, it made things easier to pass extra arguments to the command, such as Username and which server to
run things on! 

Now I am not sure what all you wanted me to put up here, so if you would like to see picture/video/show you, let me know at: dpelliccia@cardlytics.com

-Dom