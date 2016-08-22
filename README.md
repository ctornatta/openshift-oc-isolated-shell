# Overview

My attempt to solve an issue where I want to isolate my "oc" login session to my active shell. When administrating multiple OpenShift environments and your home directory is shared this can help solve the problems of all of your open terminals sharing the same "oc" login/session. This particularly solves the issue when you want to log into the environment as a cluster administrator and you don't want your oc session to be shared.

Add the following to your bash or zsh startup file:

```
# IMPROVEMENTS NEEDED: Cleanup  existing .kube-* config files if the token does not exist
find ~ -maxdepth 1 -name ".kube-*" -type f -exec bash -c 'grep -q token {} ; if [ $? -ge 1 ] ; then echo oc session kube config cleanup: {} - deleted file ; rm -f {} ; else echo oc session kube config cleanup: {} - did not delete file. Token exist; fi' \;

# Create random KUBE file per shell
export KUBE_FILE=~/.kube-`LC_CTYPE=C tr -dc a-z0-9 < /dev/urandom | head -c 4 | xargs`

# Have the oc command use this kube config file to store the token
alias oc='oc --config $KUBE_FILE'
```

The cleanup of old '~/.kube-*' can be improved by checking the token against the API.
