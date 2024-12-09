* Add some alias
* Edit this:
```
vi ~/.bashrc
```
* Add some change at the end of file:
```alias k=kubectl
alias setconfig="kubectl config set-context --current --namespace"
if [ -f /etc/profile ]; then . /etc/profile; fi
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:$(go env GOPATH)/bin
source <(kubectl completion bash)
alias k=kubectl
complete -o default -F __start_kubectl k
export GPG_TTY=$(tty)
```

