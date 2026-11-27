# Day 4: Linux Permissions

## What I learned
Every file and folder in Linux has an owner, a group, and "others". Permissions control who can read, write, or run it.

## Reading permissions with `ls -l`

```bash
ls -l
```

Example output:

```text
-rwxr-xr--  1 user devops 1200 Sep 28 deploy.sh
```

Breaking down `-rwxr-xr--`:

| Part | Who | Permissions |
|---|---|---|
| rwx | Owner | read, write, execute |
| r-x | Group | read, execute |
| r-- | Others | read only |

## Permission values

| Letter | Meaning | Number |
|---|---|---|
| r | read | 4 |
| w | write | 2 |
| x | execute | 1 |

Add the numbers for each set. For example, rwx = 4 + 2 + 1 = 7, and r-x = 4 + 1 = 5.

## Common `chmod` values

| Number | Owner | Group | Others | Typical use |
|---|---|---|---|---|
| 600 | rw- | --- | --- | Private files, credentials |
| 644 | rw- | r-- | r-- | Normal files, config files |
| 700 | rwx | --- | --- | Owner-only scripts |
| 750 | rwx | r-x | --- | Owner and group can run, others blocked |
| 755 | rwx | r-x | r-x | Executable scripts |

## Symbolic `chmod`

```bash
chmod u+x script.sh    # add execute for the owner
chmod g+w file.txt     # add write for the group
chmod o-r file.txt     # remove read from others
chmod a+x script.sh    # add execute for everyone
```

`u` = user/owner, `g` = group, `o` = others, `a` = all

## Changing owner and group

```bash
sudo chown user file.txt                # change owner
sudo chown user:group file.txt          # change owner and group
sudo chown -R user:group deployment/    # recursive, for a folder and everything in it
sudo chgrp devops deployment/           # change group only
```

Quick difference:
- `chmod` changes permissions
- `chown` changes the owner
- `chgrp` changes the group

## Who am I?

```bash
whoami    # current user
id        # user ID, group ID, and groups
groups    # groups the user belongs to
```

## Lab: deployment folder

Create the structure:

```bash
mkdir -p ~/devops-learning/linux/day04/deployment/{app,config,logs,scripts}
touch ~/devops-learning/linux/day04/deployment/app/app.py
touch ~/devops-learning/linux/day04/deployment/config/config.conf
touch ~/devops-learning/linux/day04/deployment/logs/app.log
touch ~/devops-learning/linux/day04/deployment/scripts/deploy.sh
```

Set permissions:

```bash
chmod 755 ~/devops-learning/linux/day04/deployment/scripts/deploy.sh
chmod 640 ~/devops-learning/linux/day04/deployment/config/config.conf
chmod 640 ~/devops-learning/linux/day04/deployment/logs/app.log
```

Check the result:

```bash
ls -l ~/devops-learning/linux/day04/deployment/scripts/deploy.sh
ls -l ~/devops-learning/linux/day04/deployment/config/config.conf
```

Why these values:
- `deploy.sh` is 755 so it can be run.
- `config.conf` and `app.log` are 640 so only the owner can edit them and the group can read them.

## Key takeaways
- Read `ls -l` first, before changing anything.
- Use the least permission that works. Avoid `chmod 777`.
- "Permission denied" usually means: check the owner, the group, and the permission bits.

## Mistakes and notes
- one time I forgot to change permission and got message "Permission denied" but then I used chmod command and now "Permission denied" errors stop being scary.

## Glimpse of my Lab work
<img width="1088" height="1010" alt="Screenshot 2026-11-27 105150" src="https://github.com/user-attachments/assets/563f42b4-317a-4cac-a85b-ff2b632a9540" />
