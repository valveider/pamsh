pamsh
=====

#!/bin/bash

USER=$1
SERVICE=$2

USER_UID=$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f3)
USER_GID=$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f4)
USER_HOME="$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f6)"

DOMAIN="$DOMINIO"
SERVER=$ip_servidor_de_los_homes

if [ -d ${USER_HOME} ]; then
   mount -t cifs //${SERVER}/$USER ${USER_HOME} -o user=${USER},password=${PAM_AUTHTOK},workgroup=${DOMAIN},uid=${USER_UID},gid=${USER_GID},noperm,file_mode=0600,dir_mode=0700,iocharset=utf8,nobrl,nounix 2>/dev/null
fi

exit 0


!/bin/bash

USER=$1
SERVICE=$2

USER_UID=$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f3)
USER_GID=$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f4)
USER_HOME=$(getent passwd | grep -E "^${USER}:" | cut -d ':' -f6)

if [ ! -d ${USER_HOME} ]; then
   mkdir -p ${USER_HOME} 2>/dev/null
   find /etc/skel -name ".*" -exec cp -r \{\} ${USER_HOME} \;
   chown -R ${USER_UID}.${USER_GID} ${USER_HOME} 2>/dev/null
fi

exit 0
