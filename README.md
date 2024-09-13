# Ansible Role: Gameserver

Installs game servers using SteamCMD on Linux (Ubuntu). 

Currently supported:

| Game | added on |
|------|-------|
| Palworld | 13.09.2024 |
| Satisfactory | 13.09.2024 |
| Soulmask | 13.09.2024 |

## Requirements

None.

## Role Variables

```
gameserver_adminuser: "steam"
gameserver_admingroup: "steam"
gameserver_basepath: "/home/steam/gameserver"
# which gameserver to install
gameserver_installgame: "soulmask"
```

## Example Playbook (Satisfactory)

```
- hosts: all

  vars:
    gameserver_adminuser: "steam"
    gameserver_admingroup: "steam"
    gameserver_basepath: "/home/steam/gameserver"

  roles:
    - role: gameserver
      vars:
        gameserver_installgame: "satisfactory"
        gameserver_satisfactory_enabled: true
        gameserver_satisfactory_state: stopped
```

## Example Playbook (Soulmask)

```
- hosts: all

  vars:
    gameserver_adminuser: "steam"
    gameserver_admingroup: "steam"
    gameserver_basepath: "/home/steam/gameserver"

  pre_tasks:
    - name: "create gameserver_basepath on remote instance"
      file:
        path: "/home/steam/gameserver"
        state: directory
        owner: "steam"
        group: "steam"
        mode: '0750'
      become: true
      tags: gameserver

  roles:
    - role: gameserver
      vars: 
        gameserver_installgame: "soulmask"
        gameserver_soulmask_servername: "My Super-Duper Server"
        gameserver_soulmask_maxplayers: 50
        gameserver_soulmask_serverpassword: ""
        gameserver_soulmask_adminpassword: "veryverysecret"
        gameserver_soulmask_pve: true
```

### Commands (Soulmask)

After installation log into your remote machine as the user you configured the gameserver to run. 
Start a screen session and start the gameserver in one screen using the command `soulmask_start.sh -u` 
(will try to install gameserver updates at launch time). 

For shutting down the server go to a parallel screen session and execute `telnet localhost 18888`. 
Then shutdown the server with the command `SaveAndExit 1`. 

## License

MIT / BSD

## Author Information

This role was created Stefan Pommerening as a personal project.
