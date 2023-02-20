# tanka-jsonnet

# install
brew install tanka
or
cd /tmp
go install github.com/grafana/tanka/cmd/tk@latest

brew install jsonnet-bundler
or
cd /tmp
go install github.com/jsonnet-bundler/jsonnet-bundler/cmd/jb@latest

# init
mkdir prom-grafana && cd prom-grafana # create a new folder for the project and change to it
tk init # initiate a new project

# rewrite the previous .yaml to very basic .jsonnet:

environments/default/main.jsonnet:

```jsonnet

```
# commands
tk show environments/default/main.jsonnet
tk eval environments/default/main.jsonnet

# notes
Each environment has a file called spec.json, which includes the information to select a cluster:


```spec.json
{
  "apiVersion": "tanka.dev/v1alpha1",
  "kind": "Environment",
  "metadata": {
    "name": "default"
  },
  "spec": {
    "apiServer": "https://127.0.0.1:6443", // cluster to use
    "namespace": "monitoring" // default namespace for all created resources
  }
}
```

# Tanka won't create the namespace for you -- you need to include it in Jsonnet by adding it to environments/default/main.jsonnet:
```main.jsonnet
{
  my_namespace: {
    apiVersion: "v1",
    kind: "Namespace",
    metadata: {
      name: "monitoring"
    }
  }
}
```

# commands
tk diff environments/default/main.jsonnet
tk apply environments/default/main.jsonnet 

tk delete environments/default/main.jsonnet

# demo
mkdir demo
cd demo
tk init
tk show environments/production/main.jsonnet
tk diff environments/production/main.jsonnet
tk apply environments/production/main.jsonnet 

tk delete environments/production/main.jsonnet