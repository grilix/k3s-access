# Configuración de acceso al cluster

Este rol intenta solucionar el acceso al cluster de kubernetes de una forma no destructiva,
simple y segura.

## Requerimientos

- Cluster k3s corriendo (sólo lo probé en k3s, *debería* andar con otros)
- Acceso por SSH al servidor master del cluster
- Dependencias de ansible galaxy listados en `requirements.yml`

## Instalación

Descarga el repo en el directorio `roles/`, por ejemplo:

```bash
git clone git@github.com:grilix/k3s-access.git roles/k3s-access
```

## Ejemplo de uso

```bash
# cat inventory/hosts.yml
all:
  hosts:
    server:
      ansible_host: "192.168.34.10"
      cluster_name: "my-cluster"
  children:
    k3s_master:
      server:
# cat setup-cluster-access.yml
- hosts: k3s_master
  roles:
    - k3s-access
# ansible-playbook -i inventory/hosts.yml setup-cluster-access.yml
...
```

Cuando termine de ejecutarse el playbook, deberías tener acceso usando `kubectl`:

```bash
# kubectl get nodes
NAME                   STATUS   ROLES                  AGE    VERSION
debian10.localdomain   Ready    control-plane,master   156m   v1.21.0+k3s1
```
