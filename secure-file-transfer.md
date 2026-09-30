# Secure File Transfer with SCP and SFTP

## Scope

A single disposable Linux VM used SSH loopback to demonstrate secure file transfer.
This validates SCP/SFTP mechanics locally; it does not demonstrate connectivity between separate hosts.

## Procedure

Generate a dedicated local SSH key and authorize its public key:

```bash
ssh-keygen -t rsa -b 3072 -f ~/.ssh/lab_key -N ""
cat ~/.ssh/lab_key.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
printf 'File transfer verification\n' > example.txt
scp -i ~/.ssh/lab_key example.txt "$USER@127.0.0.1:/tmp/uploaded_example.txt"
scp -i ~/.ssh/lab_key "$USER@127.0.0.1:/tmp/uploaded_example.txt" downloaded_example.txt
sftp -i ~/.ssh/lab_key "$USER@127.0.0.1"
```

Inside the SFTP session, enter only these commands (omit the `sftp>` prompt):

```text
put example.txt /tmp/sftp_uploaded.txt
get /tmp/sftp_uploaded.txt sftp_downloaded.txt
exit
```

Verify that all three hashes match:

```bash
sha256sum example.txt downloaded_example.txt sftp_downloaded.txt
```

## Privacy and evidence

This guide retains the procedure without personal details, host screenshots, or keys.
An SSH server must be running locally, and its host fingerprint should be verified before accepting it.
The original screenshot showed successful SCP transfers and invalid SFTP commands;
completed SFTP transfers and matching hashes were not demonstrated by that screenshot.
Use the commands above to obtain fresh verification evidence.
The passwordless key is intended only for a disposable local exercise.
