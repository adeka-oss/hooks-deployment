# hooks-deployment

⚓️ Otomasi sederhana menggukanan git hooks pada deployment

Panduan sederhana deploy menggunakan hooks dari git, sebagai alternatif ftp.


## Kebutuhan
- Dasar penggunaan git
- Dasar penggunaan terminal

## Latar Belakang

## Bagaimana ini bekerja?

Pada kasus ini menambahkan perintah ["bare"](https://www.geeksforgeeks.org/bare-repositories-in-git/) pada repositori git yang diletakkan pada server dan untuk mempublikasikan branch master(sekarang main)  secara langsung mempublikasikannya ke dalam server.

## Langkah - langkah

1. Membuat folder untuk production di server seperti pada umumnya di dalam web server. Seperti halnya /var/www

2. Menambahkan bare repositori pada server *production*

3. Membuat *script* **post-receive** untuk melakukan hook dari bare repository di server (Jangan lupa scriptnya executable 😘)

4. Menambahkan *remote repository* ke production server yang ada di repositori lokal.

5. Kemudian tinggal push dan santuy.


# 1. Membuat folder pada server
```
$ ssh elitglobal@ganteng.com
$ mkdir ~/deploy-folder-kamu
```

# 2. Menambahkan bare repositori pada server
Seperti menginisiasikan proyek git pada umumnya. Dan beri nama sesuai selera:
```
$ git init --bare ~/project.git
```
# 4. Membuat skrip yang dibuat agar executable
```
chmod +x post-receive
```

Isi file **post-receive**

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

# 5. Menambahkan remote-repository lokal untuk production server
```
$ cd ~/path/to/working-copy/
$ git remote add production demo@yourserver.com:project.git
```
# 6. Melakukan push pada production server

```
git push production master

```

## Credit
- [noelboss](https://gist.github.com/noelboss/3fe13927025b89757f8fb12e9066f2fa)
