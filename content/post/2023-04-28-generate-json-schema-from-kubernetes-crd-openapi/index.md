---
date: 2023-04-28T12:00:00
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

Then I came across a [blog post by Sean Liao](https://seankhliao.com/blog/12021-08-07-json-schema-openapi-kubernetes-crd/), it was somewhat thin on details, but gave me just enough to get started.

Since I already have a Kubernetes cluster with the version of ArgoCD that we use installed, I can use the OpenAPI endpoint to get the OpenAPI spec for the CRD.

```shell
kubectl get --raw /openapi/v3
```

This returned a collection of specs each with a `serverRelativeURL`.  Rather than doing any fancy filtering I simply grep'd for `argocd` and grabbed the URL, and reissued by request:

```shell
kubectl get --raw '/openapi/v3/apis/argoproj.io/v1alpha1?hash=<huge hash>' | jq '.' > argo.json
```

I then used [openapi2jsonschema](https://github.com/yannh/openapi2jsonschema) to convert the OpenAPI spec to JSON Schema:

```shell
openapi2jsonschema argo.json
```

This places all of the schemas in a directory called `schemas`.  
I was then able to copy the schema for the ArgoCD Application over the top of our old one to be used in CI.
