# SETUP OF COUCHDB ON MYBONK STACK

If you are really new to computing you may benefit from consulting our "[Baby rabbit holes](https://github.com/mybonk/mybonk-wiki/blob/main/baby-rabbit-holes.md)".

If you want to learn more about how the "automagic" installation process works under the hood have a look at our [MYBONK Wiki](https://github.com/mybonk/mybonk-wiki/tree/main).

This is based on MYBONK stack, a node module for NixOS [https://github.com/mybonk/mybonk](https://github.com/mybonk/mybonk).

The present instructions and code are stored in [https://github.com/we-do-it-lu/mybonk-couchdb](https://github.com/we-do-it-lu/mybonk-couchdb).

[What is Apache Couchdb](https://www.youtube.com/watch?v=aOE90VAVOcU)
[What is Obsidian](TODO)
[What is Obsidian Live-Sync?](TODO)

This is not a full instructions on how to setup a complete MYBONK full node but rather demonstrate how a service like coucdb can be installed, configured and managed on it. As always we will mostly use the following commands to interact with the node:
- `mybonk-erase-and-install.sh`  (if you don't have a node to play with yet).
- `mybonk-rebuild.sh`: To rebuild the target system with the updated configuration.
- `mybonk-term-open.sh` To open a pre-configured tmux terminal on the target system.
- `mybonk-term-kill.sh` To kill a pre-configured tmux terminal on the target system.

Thereby the configuration that will be used is the simplest you can imagine (only basic services and Tailscale), it is refered to as '.#generic' in this document. 

Any feedback is welcome.

## INSTRUCTIONS



### STEP 1
On any NixOS system clone the present repository:
```
$ git clone https://github.com/we-do-it-lu/mybonk-couchdb.git
$ cd mybonk-couchdb
````

### STEP 2
In the cloned directory modify the `configuration.nix` to set your own ssh public keys (parameters `authorizedKeys.keys`) instead of the ones there by default which are only examples to show you what it looks like. Again: Make 100% sure you put your own key(s) (public key of a private-public keys pair) else you may loose access to the machine after installation.

### STEP 3
Configure your couchdb instance to suit your needs.
You can inspire yourself from this article [https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md](https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md)

Please refer to the [official document of couchdb](https://docs.couchdb.org/en/stable/install/index.html), however, we do not have to configure it fully. Only the administrator needs to be configured for Obsidian-livesync to work.

This is done in `couchdb.nix`, open it and set values for the parameters `adminUser` and `adminPassword`.

### STEP 4
Rebuild the target system:
```
$ ./mybonk-rebuild.sh switch --target-host root@178.156.170.26 --flake .#generic
```

### STEP 5

Now we need to initialise couchdb so that it is ready to run Live-Sync using a ready-made script but first we need to set the parameters `adminUser` and `adminPassword` as you configured them in `couch-db.nix`:



`
export username=FIXME
export password=kpkdasdosakpdsa #Please change as you like
curl -s https://raw.githubusercontent.com/vrtmrz/obsidian-livesync/main/utils/couchdb/couchdb-init.sh | bash
`

If it results like the following:

`
-- Configuring CouchDB by REST APIs... -->
{"ok":true}
""
""
""
""
""
""
""
""
""
<-- Configuring CouchDB by REST APIs Done!
``

### STEP 6
Done. 

Your target server is now running couchdb. 

### CONCLUSION

Now you can configure your Obsidan client to use you couchdb in the back-end!

