> A typical helpful post-installation configuration is to enable shell autocompletion for kubectl. For bash shell it can be achieved by running the following sequence of commands:
```bash
sudo apt update && sudo apt install -y bash-completion

source /usr/share/bash-completion/bash_completion

source <(kubectl completion bash)

echo 'source <(kubectl completion bash)' >> ~/.bashrc
```


> In ~/.bashrc add an alias and use it for completion
```bash
# kubectl alias
alias k=kubectl

# kubectl completion for k alias
source <(kubectl completion bash)
complete -F __start_kubectl k
```