# CI

## Tips

### SSHの鍵を登録する


`ssh-keygen -t rsa -b 4096 -N '' -C "gitlab-ci@xxx"`

```
if [[ ! -d ~/.ssh ]]; then
    mkdir -p ~/.ssh
fi

if [[ -z "$PRIVATE_KEY" ]]; then
    echo ""
    echo "Do not deploy the rpm to rpm.stars.axelspace.com."
    echo "Because PRIVATE_KEY is not defined"
    exit 1
fi

echo "$PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
chmod 600 ~/.ssh/id_rsa
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
ssh-keyscan -p 777 rpm.stars.axelspace.com >> ~/.ssh/known_hosts

```


