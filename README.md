Energetic Pixels' Single Instance MongoDB
====================================================

### FOR DEVELOPMENT ONLY.  NOT SETUP FOR PRODUCTION ENVIRONMENT!!!

* Simple vagrant box for Ubuntu 18.0.4 operating system.
* Port mirroring between Host 27017 and VM 27017.
* VM can be IP accessed at 192.168.33.10.
* MongoDB instance version 4.x.
  * User Admin profile (username and password) hardcoded.
  * User DB user profile (username and password) to project db hardcoded
  * with readWrite role.
  * Listening on port 27017.

```
1. Install VirtualBox 4.0 (minimum version).
2. Install Vagrant (min version 2)
3. Clone this repo to local folder on your hard-drive
4. Using a terminal window, change directory to your new local folder (step 3)
5. User of this VM will need to setup a regular db (collection) user. Attach code below to line 53 and modify its db reference.
6. Using the same terminal window, type "vagrant up --provision"
```

```
,
{
  user: 'reg_user', 
  pwd: 'password', 
  roles: [{
    role: 'readWrite', 
    db: 'somedatabasehere'}]
}
```
