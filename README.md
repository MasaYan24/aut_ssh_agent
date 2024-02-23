# auto ssh-agent

## code
```sh
function auto_ssh_agent() {
    result=$(find /tmp -type d -name 'ssh-XXXXXX*' -printf '%T@ %p\n' | sort -n | tail -n 1)
    t=$(echo "$result" | awk '{print $1}')
    p=$(echo "$result" | awk '{print $2}')
    f=$(find $p -name 'agent*')
    agent_pid=$(expr ${f#*agent.} + 1)
    ps -p $(echo $agent_pid) > /dev/null
    if [ $? -eq 0 ]; then
        export SSH_AUTH_SOCK=$f
    else
        eval "$(ssh-agent)" 1> /dev/null
    fi
}
```

## how to use
```sh
auto_ssh_agent
```
