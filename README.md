# hooks-deployment
⚓️ Simple automated GIT Deployment using GIT Hooks

## Prerequisit
- basic usage git
- basic terminal usage

## Todo

1.Create a folder to deploy to on production server (i.e. your httpds folder)

2.Add a bare repository on the productions server

3.Add the post-receive hook script to the bare repository (and make it executable)

4.Add the remote-repository resided on the production server to your local repository

5.Push to the production server, relax.

# 1. Have a local working-working copy ready
```
$ ssh user@server.com
$ mkdir ~/deploy-folder
```
# 2. Create a folder to deploy to

# 3. Add a bare repository on the productions server
```
$ git init --bare ~/project.git

```
# 4. Add the post-receive hook script
```
chmod +x post-receive
```
# 5. Add remote-repository localy
```
$ cd ~/path/to/working-copy/
$ git remote add production demo@yourserver.com:project.git
```
# 6. Push to the production server

```
git push production master

```

post-receive

```bash
#!/bin/bash
TARGET="/home/webuser/deploy-folder"
GIT_DIR="/home/webuser/www.git"
BRANCH="master"

while read oldrev newrev ref
do
	# only checking out the master (or whatever branch you would like to deploy)
	if [ "$ref" = "refs/heads/$BRANCH" ];
	then
		echo "Ref $ref received. Deploying ${BRANCH} branch to production..."
		git --work-tree=$TARGET --git-dir=$GIT_DIR checkout -f $BRANCH
	else
		echo "Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server."
	fi
done
```

## Todo
- Coretan ✅
- Bikin dokumentasi
- Translet ke ID

## Credit
- [noelboss](https://gist.github.com/noelboss/3fe13927025b89757f8fb12e9066f2fa)
