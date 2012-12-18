cfengine-bacula
===============

Cfengine3 bundle to manage all Bacula configuration.

Description
==============
This tool was made to automate Bacula's configuration. Bacula can have a huge 
number of configuration files for each client, job, schedule. This tool aims to
make it easy to add/remove a job using [Amanda|www.amanda.org]'s syntax:

```
myfirstserverdomain.local   /etc    standard
myfirstserverdomain.local   /home   standard
mysecondserverdomain.local  /etc    standard
mysecondserverdomain.local  /home   standard
```

How to
==============
Here is the promise.cf file needed for cfengine to handle bacula.cf:
```
body common control
{
    bundlesequence  => { "app_bacula" };

    inputs          => {
        "cfengine_stdlib.cf",
            "libdefault.cf",
            "bacula.cf"
    };
}

bundle common globals{
classes:
    "backupmaster" or => { "backmaster_mydomain_local" };
    "backupclient" or => { 
        "client1_mydomain_local",
        "client2_mydomain_local",
        "client3_mydomain_local"
    };

    "cfmaster" or => { "cfengine_mydomain_local"};

vars:
    any::
        # CFengine server's IP address
        "serverhost"    string => "192.168.122.12";

        # My file repository on cfengine's server
        "group_path"    string => "/data/cf-repos";

        # For etc files:
        "etc_src"       string => "$(group_path)/default/etc";
}

body agent control {
# Do not rely on DNS
        skipidentify => "true";
}
```



Links
=============
* Cfengine's website : http://cfengine.com
* Bacula's website : http://www.bacula.org


