PAM (Pluggable Authentication Modules) are probably the most used auth mechanism in linux, as the "normal" login + a ton of other applications use it per default.

PAM services files are located in /etc/pam.d/, and look something like this:
```
#%PAM-1.0
auth     required  pam_securetty.so
auth      include   system-remote-login
account   include   system-remote-login
password  include   system-remote-login
session   include   system-remote-login
```

Each line is independent, and usually go like this:
`<interface> <control flag> <module>`
or
`<interface> <control flag> <module> <optional arguments>`

### interface
The interface has 4 possible values:
* `auth` : Authenticate use, from the example one above, our first auth module makes sure we aren't root, and the 2nd one runs the service "system-remote-login", which runs "system-login", which checks that the user is valid, has a valid shell, isn't no login and all that.
* `account` : Authenticate access, Alright, so we're a valid account (as auth passed), lets check if we're actually allowed to login.
* `password` : Check password, usually done against /etc/shadow, but you could also have a pam module which posts to some webserver
* `session` : Configure and manage user session, example show MOTD, set env and check mails

### control flag
The control flag also has 4 possible values:
- `required` :  If module fails, authentication is failed, although the user is not notified until the rest of the modules on this interface are done
- `requisite` : If module fails, authentication is immediately failed, and the user is notified
- `sufficient` : If module fails, it is ignored, but if it is successful, and no required module above has failed, then the user is accepted
- `optional` : If module fails, it is ignored, if it is successful, cool

### module
A module is a .so file, which takes a PAM_HANDLE, and has a method akin to
```c
PAM_EXTERN int pam_sm_authenticate( pam_handle_t *pamh, int flags,int argc, const char **argv ) {
	return PAM_SUCCESS
}
```

(this method is used in the auth interface)

PAM_CONV is made avaliable for the module in case the user has to be notified of anything (why they failed, etc.).

