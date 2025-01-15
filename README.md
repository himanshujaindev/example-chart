# example-chart

---

example-chart -> For argocd

---
kind create cluster --name helm --config kind.yaml
https://helm.sh/docs/chart_template_guide/getting_started/
helm create mychart
helm install myname ./mychart
helm get manifest myname
helm uninstall myname

helm install --debug --dry-run myname ./mychart
helm install --debug --dry-run myname ./mychart --set favoriteDrink=slurm
helm install --debug --dry-run myname ./mychart --set favorite.food=null


kind delete cluster --name helm

---
- A template directive is enclosed in {{ and }} blocks.
- read .Release.Name as "start at the top namespace, find the Release object, then look inside of it for an object called Name" (namespaced objects)
- Objects = Had just one value (can be another object or function)
- Example Objects:
    Release
    Values -> values.yaml
    Chart -> Chart.yaml
    Files -> access to all non-special files
    Capabilities -> information about capabilities of Kubernetes cluster
    Template
-  recommendation is that you keep your values trees shallow, favoring flatness (values.yaml)

paused at functions_and_pipelines