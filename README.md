# SETUP OF COUCHDB ON MYBONK STACK

If you are really new to computing you may benefit from consulting our "[Baby rabbit holes](https://github.com/mybonk/mybonk-wiki/blob/main/baby-rabbit-holes.md)".

If you want to learn more about how the "automagic" installation process works under the hood have a look at our [MYBONK Wiki](https://github.com/mybonk/mybonk-wiki/tree/main).

The deployment and test process is described in [https://github.com/mybonk/mybonk](https://github.com/mybonk/mybonk).

The present instructions and code are stored in [https://github.com/we-do-it-lu/mybonk-couchdb](https://github.com/we-do-it-lu/mybonk-couchdb).

This is not a full instructions on how to setup a complete MYBONK full node but rather demonstrate how a service like coucdb can be installed, configured and managed on it. As always we will mostly use the following commands to interact with the node:
- `mybonk-erase-and-install.sh`  (if you don't have a node to play with yet).
- `mybonk-rebuild.sh`: To rebuild the target system with the updated configuration.
- `mybonk-term-open.sh` To open a pre-configured tmux terminal on the target system.
- `mybonk-term-kill.sh` To kill a pre-configured tmux terminal on the target system.

Thereby the configuration that will be used is the simplest you can imagine (only basic services and Tailscale), it is refered to as '.#generic' in this document. 

Any feedback is welcome.

## INSTRUCTIONS



### STEP 1
On any NixOS system (or at least any system having Nix, the package manager, installed) clone the present repository:
```
$ git clone https://github.com/we-do-i/mybonk-couchdb.git
$ cd mybonk-couchdb
````

### STEP 2
In the cloned directory modify the `configuration.nix` to set your own ssh public keys (parameters `authorizedKeys.keys`) instead of the ones there by default which are only examples to show you what it looks like. Again: Make 100% sure you put your own key(s) (public key of a private-public keys pair) else you may loose access to the machine after installation.

### STEP 3
Configure your couchdb instance to suit your needs.
You can inspire yourself from this article [https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md](https://github.com/vrtmrz/obsidian-livesync/blob/main/docs/setup_own_server.md)

### STEP 4
Rebuild the target system:
```
$ ./mybonk-rebuild.sh switch --target-host root@178.156.170.26 --flake .#generic
```

### STEP 5
Done. 

Your target server is now running couchdb. 

### CONCLUSION

Now you can configure your Obsidan client to use you couchdb in the back-end!

Exercise yourself connecting and diconnecting to you node using the following commands:

#### mybonk-term-open.sh
- Allows you to open a preconfigured tmux session with MYBONK. 
- Use the option `--help` to see all that is possible.
```
$ ./mybonk-term-open.sh operator@178.156.170.26 --remote-dir mybonk
```
- The option `--remote-dir`is very important here, it tells the script where to find the tmuxinator settings, typically the directory where you cloned the repository (in this example it is `mybonk`). It is not needed if you run the script locally (without the optional `--remote-dir` parameter).

#### mybonk-term-close.sh
- Kill the tmux session with MYBONK. 
- Use the option `--help` to see all that is possible.
```
./mybonk-term-close.sh operator@178.156.170.26
```
