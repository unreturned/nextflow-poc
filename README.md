Создадим namespace

```
kubectl create namespace nextflow-poc
```

Создадим SA

```
kubectl create -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nextflow-runner
  namespace: nextflow-poc
EOF
```

Дадим права

```
kubectl create -f - <<EOF
apiVersion: deckhouse.io/v1
kind: ClusterAuthorizationRule
metadata:
  name: nextflow-runner
spec:
  subjects:
  - kind: ServiceAccount
    name: nextflow-runner
    namespace: nextflow-poc
  accessLevel: Editor
EOF
```

Дополнительно дадим возможность работы с подами Editor

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    user-authz.deckhouse.io/access-level: Editor
  name: user-editor-create-pods
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - pods/status
  verbs:
  - get
  - list
  - watch
  - create
  - delete
  - deletecollection
  - patch
  - update
```