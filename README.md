# BootupCtl
Controlling Linux Bootup Programs in a Elegant Way.

## Introduction
Have you ever been troubled with adding or deleting bootup items in linux?

Now, you have a complete solution.

BootupCtl provides a CLI-Interface and an easy-to-use RESTful API.
You can run it as a daemon, or just manage from scripts.

Written in Golang, so it's cross-platform very high-performance.

## Quick View
```
user@Ubuntu:~$ sudo bootupctl add someReactPHPDaemon "/usr/bin/php /srv/someProject/someScript/daemon.php" --backend "init.d"
[Info][2018/4/29 18:38:01] +============================+
[Info][2018/4/29 18:38:01] |  BootupCtl v0.1.0-alpha    |
[Info][2018/4/29 18:38:01] | Made with love by xtlsoft. |
[Info][2018/4/29 18:38:01] +============================+
[Info][2018/4/29 18:38:01] Backend: init.d
[Info][2018/4/29 18:38:01] 
[Info][2018/4/29 18:38:01] Please Wait......
[Info][2018/4/29 18:38:01] 
[Info][2018/4/29 18:38:02] Added "someReactPHPDaemon".
[Info][2018/4/29 18:38:02] 

user@Ubuntu:~$ sudo bootupctl daemon start
[Info][2018/4/29 18:38:11] +============================+
[Info][2018/4/29 18:38:11] |  BootupCtl v0.1.0-alpha    |
[Info][2018/4/29 18:38:11] | Made with love by xtlsoft. |
[Info][2018/4/29 18:38:11] +============================+
[Info][2018/4/29 18:38:11] Configure: /etc/bootupctl.json
[Info][2018/4/29 18:38:11] 
[Info][2018/4/29 18:38:11] Please Wait......
[Info][2018/4/29 18:38:11] 
[Info][2018/4/29 18:38:12] Started daemon at http://0.0.0.0:28491/.
[Info][2018/4/29 18:38:11] RESTful Address: http://0.0.0.0:28491/v1/
[Info][2018/4/29 18:38:12]

user@Ubuntu:~$ sudo bootupctl list
+====================+=====================================================+
|        Name        |                        Script                       |
+====================+=====================================================+
| BootupCtlDaemon    | /usr/bin/bootupctl daemon start                     |
+--------------------+-----------------------------------------------------+
| someReactPHPDaemon | /usr/bin/php /srv/someProject/someScript/daemon.php |
+--------------------+-----------------------------------------------------+

```
```php
<?php
    require_once "vendor/autoload.php";

    use \CloudSky\BootupCtl\Client as BootupCtlClient;

    $client = new BootupCtlClient("http://Ubuntu.local:28491/v1/");

    echo json_encode([
        $client->list(),
        $client->add("someOtherDaemon", "/usr/bin/someOtherDaemon start"),
        $client->remove("someReactPHPDaemon"),
    ], \JSON_PRETTY_PRINT);
    /* Result:
        [
            [
                "BootupCtlDaemon" => "/usr/bin/bootupctl daemon start",
                "someReactPHPDaemon" => "/usr/bin/php /srv/someProject/someScript/daemon.php"
            ],
            true,
            true
        ]
    */


```