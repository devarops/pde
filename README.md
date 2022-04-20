# Configura tu entorno para desarrollo

## Autenticación mediante SSH

1. En tu estación de trabajo crea tu clave SSH con: `ssh-keygen`
1. Agrega la clave SSH pública[^ssh_pub] de tu estación de trabajo a:
    - [Bitbucket](https://bitbucket.org/account/settings/ssh-keys/),
    - [DigitalOcean](https://cloud.digitalocean.com/account/security) y
    - [GitHub](https://github.com/settings/keys/)

[^ssh_pub]: Copia el contenido del archivo `~/.ssh/id_rsa.pub` de tu estación de trabajo y pégalo en las aplicaciones indicadas

## En DigitalOcean

1. [Crea una Droplet](https://cloud.digitalocean.com/droplets/new)
    - Selecciona la región correspondiente a la IP flotante (actualmente es San Francisco 3: SFO3)
    - Selecciona las claves SSH de todos los miembros del equipo como medio de autenticación
    - En el campo _Choose a hostname_, escribe `devserver`:
 
![image](https://user-images.githubusercontent.com/35377740/164117896-95a0edb4-c59a-42cc-855f-0745d591321c.png)

1. [Reasigna la IP](https://cloud.digitalocean.com/networking/floating_ips) flotante correspondiente a la Droplet nueva

## En tu estación de trabajo

> NOTA: En realidad esto no tiene que ser en tu estación de trabajo, podría ser desde otra estación de trabajo o servidor mediante Terraform.

1. Agrega o actualiza la bóbeda secreta de tu estación de trabajo
1. Instala Ansible y Make: `sudo apt update && sudo apt install --yes ansible make`
1. Crea el archivo `/etc/ansible/hosts`[^ansible_hosts]
1. En la raiz del repo [`development_server_setup`](https://github.com/IslasGECI/development_server_setup) corre el _playbook_[^playbook]

[^ansible_hosts]: El contenido del archivo `/etc/ansible/hosts` es el siguiente:
    ```
    [development]
    islasgeci.dev ansible_user=root
    ```

[^playbook]: En la raiz del repo [`development_server_setup`](https://github.com/IslasGECI/development_server_setup) ejecuta:
    ```shell
    ssh-keygen -f "${HOME}/.ssh/known_hosts" -R "islasgeci.dev"
    ANSIBLE_HOST_KEY_CHECKING=False && make
    ```

## En el servidor de desarrollo

> TODO: Mueve esta sección al _playbook_ que se encuentra en [Development server setup via Ansible](https://github.com/IslasGECI/development_server_setup)

1. Entra mediante: `ssh root@islasgeci.dev`
1. Instala [geci-testmake](https://github.com/IslasGECI/testmake)
1. Instala los [archivos de configuración personal](https://github.com/devarops/dotfiles/blob/develop/README.md)
1. Agrega tu nombre y correo en Git: `git config --global --edit`

---

# Crea un entorno para desarrollo en un contenedor

Ejecuta:

```shell
docker pull evaristor/pde:latest
docker run --interactive --name devenv --rm --tty --volume ${PWD}:/workdir evaristor/pde:latest bash
```

# Related repos

- [Internal Developer Platform (IDP)](https://github.com/IslasGEI/devenv)
- [Dotfiles](https://github.com/devarops/dotfiles)
- [Development server setup via Ansible](https://github.com/IslasGECI/development_server_setup)
