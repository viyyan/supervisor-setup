## Installing Supervisor for Laravel job worker

This is step by step install Supervisor and running worker program.

-----------------

* Install **supervisor**
 
```shell
$ sudo pip install supervisor
$ echo_supervisord_conf
$ sudo su -
$ echo_supervisord_conf > /etc/supervisord.conf
```
* Set our program files directory 

```
$ sudo mkdir /etc/supervisor/conf.d
$ sudo nano /etc/supervisord.conf

[include]
files = /etc/supervisord/conf.d/*.conf
```
* Create init script to run supervisor `/etc/init.d/supervisord`. Script that I used running on AWS AMI [supervisord_start](/supervisord_startup). 
Here is another resource [init script](https://github.com/Supervisor/initscripts) for various Linux Distros 
* Make it executable

```shell
$ chmod a+x /etc/init.d/supervisord
$ sudo chmod +x /etc/rc.d/init.d/supervisord
$ sudo chkconfig --add supervisord
$ sudo chkconfig supervisord on
```
* Start supervisor 
`$ sudo service supervisord start`
* Create laravel worker file 
`$ sudo nano /etc/supervisor/conf.d/laravel-worker.conf`
* Add following contents to the worker file [laravel-worker.conf](/example.conf) 
* Create **log file for supervisor** `touch /var/log/supervisor/supervisord.log` (if not exist)
* **Restart** the supervisord `$ sudo supervisord restart`
* **Reread** supervisorctl new program `$ sudo /usr/local/bin/supervisorctl reread`
* **Update** supervisorctl `$ sudo /usr/local/bin/supervisorctl update` 
* **Start** laravel-worker and any other workers as `$ sudo /usr/local/bin/supervisorctl start laravel-worker:*`
* **Check status** whether workers are running as `$ sudo /usr/local/bin/supervisorctl status` 

```log
laravel-worker:laravel-worker_00   RUNNING   pid 10039, uptime 0:20:35
laravel-worker:laravel-worker_01   RUNNING   pid 10040, uptime 0:20:35
laravel-worker:laravel-worker_02   RUNNING   pid 10041, uptime 0:20:35
laravel-worker:laravel-worker_03   RUNNING   pid 10042, uptime 0:20:35
laravel-worker:laravel-worker_04   RUNNING   pid 10043, uptime 0:20:35
```
* **Restart** laravel-worker and any other workers `$ sudo /usr/local/bin/supervisorctl restart laravel-worker:*`
