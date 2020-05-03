<<<<<<< HEAD
Nomand Sandbox
=======
Nomad Consul Docker Redis Vagrant Sandbox
>>>>>>> dev
=========

Travis (.com) dev branch:
[![Build Status](https://travis-ci.com/githubfoam/nomad-sandbox.svg?branch=dev)](https://travis-ci.com/githubfoam/nomad-sandbox)  


             ~~~~

             - nomad agent -config /vagrant/server.hcl &>/dev/null &
             # This means that this server will manage state and make scheduling decisions but will not run any tasks
             # start an agent in server only mode and have it elected as a leader             
             - nomad server members
             # view the members of the gossip ring
             - nomad agent -config /vagrant/client1.hcl &>/dev/null &
             - nomad agent -config /vagrant/client2.hcl &>/dev/null &
             # need some agents to run tasks
             - nomad node status
             # see the registered nodes of the Nomad cluster
             - nomad job init
             # use the job init command which generates a skeleton job file
             # a single task 'redis' which is using the Docker driver to run the task
             - nomad job run example.nomad
             # register example job
             - nomad status example
             # inspect the status of  example job
             - curl -Is http://localhost:4646 | head -n 1
             # Opening the Web UI
             # hosted at the same address and port as the Nomad HTTP API under the /ui namespace.
             # http://localhost:4646/ui/clients
             # HTTP/1.1 307 Temporary Redirect

             ~~~~

             ~~~~

             KVM/Installation
             https://help.ubuntu.com/community/KVM/Installation

<<<<<<< HEAD
An optional section for the role authors

~~~~
Jobs
https://learn.hashicorp.com/nomad/getting-started/jobs
~~~~
=======
             ~~~~
>>>>>>> dev
