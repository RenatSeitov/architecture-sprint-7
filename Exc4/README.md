## Ролевой доступ к Kubernetes

### Таблица ролей

| Роль                    | Права роли                                              | Группы пользователей                          |
|-------------------------|--------------------------------------------------------|-----------------------------------------------|
| **viewer**             | Просмотр всех ресурсов в кластере                      | Группа аналитиков                              |
| **editor**             | Управление настройками ресурсов кластера (без удаления)| Группа DevOps                                  |
| **admin**              | Полный доступ к управлению кластером, включая секреты  | Группа системных администраторов              |

### Скрипты для создания пользователей
```bash
# Создание пользователей
kubectl create serviceaccount user-viewer -n kube-system
kubectl create serviceaccount user-editor -n kube-system
kubectl create serviceaccount user-admin -n kube-system
```

### Скрипты для создания ролей
```yaml
# Роль viewer
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: kube-system
  name: viewer
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["get", "list", "watch"]

---

# Роль editor
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: kube-system
  name: editor
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["get", "list", "watch", "create", "update", "patch"]

---

# Роль admin
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: kube-system
  name: admin
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
```

### Скрипты для привязки пользователей к ролям
```bash
# Привязка пользователя viewer
kubectl create rolebinding viewer-binding \
  --role=viewer \
  --serviceaccount=kube-system:user-viewer \
  --namespace=kube-system

# Привязка пользователя editor
kubectl create rolebinding editor-binding \
  --role=editor \
  --serviceaccount=kube-system:user-editor \
  --namespace=kube-system

# Привязка пользователя admin
kubectl create rolebinding admin-binding \
  --role=admin \
  --serviceaccount=kube-system:user-admin \
  --namespace=kube-system
```

