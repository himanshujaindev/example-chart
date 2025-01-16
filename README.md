# example-chart

---

example-chart -> For argocd

---
kind create cluster --name helm --config kind.yaml

helm create mychart
helm install --debug --dry-run=server myname ./mychart
helm install myname ./mychart
helm get manifest myname
helm uninstall myname

helm install --debug --dry-run myname ./mychart
helm install --debug --dry-run myname ./mychart --set favoriteDrink=slurm
helm install --debug --dry-run myname ./mychart --set favorite.food=null
helm install --debug --dry-run=server myname ./mychart (When using lookup function as it require access k8s api)
helm install --debug --dry-run=server --disable-openapi-validation myname ./mychart

kind delete cluster --name helm

```
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
- Template functions follow the syntax -> functionName arg1 arg2...
- pipeline -> Invert -> arg1 | functionName
  result of the first evaluation is sent as the last argument to the function
- Helm template language = combination of the Go template language, some extra functions, and a variety of wrappers to expose certain objects to the templates
- lookup function can be used to look up resources in a running cluster
- Control structures are called "actions"
  if/else
  with = controls variable scoping
         . is a reference to the current scope. So .Values tells the template to find the Values object in the current scope
         $ is mapped to the root scope
  range
    |- marker in YAML takes a multi-line string
- named template (partial or subtemplate) - define, template, block
  template names are global
  files whose name begins with an underscore (_) are assumed to not have a manifest inside
  include = pass the output to another pipeline function (used instead of template object)
  define does not produce output unless it is called with a template
  It is considered preferable to use include over template in Helm templates simply so that the output formatting can be handled better for YAML documents.
- When the template engine runs, it removes the contents inside of {{ and }}, but it leaves the remaining whitespace exactly as is
- {{- (with the dash and space added) indicates that whitespace should be chomped left, while -}} means whitespace to the right should be consumed. Be careful! Newlines are whitespace!
    {{- 3 }} = trim left whitespace and print 3
    {{-3 }} = print -3
- variable is a named reference to another object

- Helm Debugging
    helm lint mychart
    helm template --debug mychart
    helm install --dry-run --debug myname mychart
    helm install --dry-run=server --debug myname mychart

    helm install myname ./mychart
    helm get manifest myname
    helm uninstall myname
```

if/else:
```
{{ if PIPELINE }}
  # Do something
{{ else if OTHER PIPELINE }}
  # Do something else
{{ else }}
  # Default case
{{ end }}
```

with:
```
{{ with PIPELINE }}
  # restricted scope
{{ end }}
```

define:
```
{{- define "MY.NAME" }}
  # body of template here
{{- end }}
```

Reference:
1. https://helm.sh/docs/chart_template_guide/getting_started/
2. https://helm.sh/docs/chart_template_guide/function_list/
3. https://pkg.go.dev/text/template
4. https://masterminds.github.io/sprig/