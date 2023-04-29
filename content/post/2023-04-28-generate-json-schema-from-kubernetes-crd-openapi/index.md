---
date: 2023-04-28T12:17:00
title: "Generate JSON Schema from Kubernetes CRD OpenAPI"
tags: ["kubernetes", "openapi", "json schema"]
categories: ["computing"]
---

At work we use [ArgoCD](https://argoproj.github.io/cd/).  One of the things we do in our CI is to check that some files modified by our users are actually valid.

ArgoCD Applications have been modified in various versions over the past year, and our internal json schema was out of date, so I wanted to update it.  
But where did the original schema come from?  I couldn't find it in the ArgoCD repo, so I guess someone must have crafted it by hand, until I saw it was several thousand lines long!

Lots of people are off work today, so I couldn't ask anyone how this was generated before.  So off I went to search the internet for "How to convert a Kubernetes CRD OpenAPI to JSON Schema".  
I didn't find many helpful results.

I tried ChatGPT, but each program it generated either errored or produced invalid JSON.  I tried a few other tools, but they all had the same problem.

Then I came across a [comment on a GitHub issue](https://github.com/yannh/kubeconform/issues/51#issuecomment-880223640) with the information I needed!.

I had already tried [https://github.com/yannh/openapi2jsonschema](https://github.com/yannh/openapi2jsonschema) and had some errors running it, but this comment made reference to another copy of the python script.  So I went ahead and downloaded that, then ran it:

```shell
curl -s -L "https://raw.githubusercontent.com/yannh/kubeconform/master/scripts/openapi2jsonschema.py" -o openapi2jsonschema.py
python3 ./openapi2jsonschema.py https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/crds/application-crd.yaml
```

This script outputted the following:

```
JSON schema written to application_v1alpha1.json
```

Indeed, looking in my current working directory I do see a file called `application_v1alpha1.json`.  
This has now been copied over the top of our old one to be used in CI.
