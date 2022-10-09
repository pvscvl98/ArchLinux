# gpg + pass

## gpg - GNU Privacy Guard

generate gpg key
```
gpg --full-gen-key
```

list secret keys
```
gpg --list-secret-keys
```

list keys
```
gpg --list-secret-keys
`````

remove keys
```
gpg --delete-secret-keys
gpg --delete-keys
```

encrypt 'doc.txt' for luke@archwiki.org (asymmetric)
```
gpg --encrypt --recipient luke@archwiki.org doc.txt
```

encrypt 'doc.txt' with only a passphrase (symmetric)
```
gpg --decrypt doc.txt.gpg
```

---

## pass

List the whole store tree:
```
pass
```
```
Password Store
└── archlinux.org
    └── wiki
        └── username
```

Initialize (or re-encrypt) the storage using one or more GPG IDs:
```
pass init gpg_id_1 gpg_id_2 gpg_id_3
```

Add an entry:
```
pass add archwiki.org/wiki/username
```

Edit an entry:
```
pass edit path/to/data
```

Generate a new random password with a given length, and copy it to the clipboard:
```
pass generate -c path/to/data num
```

pass dmenu wrapper:
```
passmenu
```
