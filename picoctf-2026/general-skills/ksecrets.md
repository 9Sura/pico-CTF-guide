# KSECRETS PROBLEM GUIDE:

*(General Skills — Easy, 100pt)*

## HINTS:
Hint 1: Kubernetes Secrets are only Base64-encoded, not encrypted.
Hint 2: Check every namespace, not just default.

## TOOLS:
`$ kubectl --kubeconfig=kubeconfig.yaml get secrets -A`

`$ kubectl ... get secret <name> -n <ns> -o yaml`

`$ echo <b64> | base64 -d`

## WALKTHROUGH:
1. Edit `kubeconfig.yaml` so the `server:` line points at the challenge URL (e.g. `https://green-hill.picoctf.net:<port>`).

2. `$ kubectl --kubeconfig=kubeconfig.yaml --insecure-skip-tls-verify=true get secrets`
    - `No resources found in default namespace.` — look wider

3. `$ kubectl ... get secrets -A`
    - `-A` = all namespaces. A `ctf-secret` shows up in the `picoctf` namespace

4. `$ kubectl ... get secret ctf-secret -n picoctf -o yaml`
    - `data: flag: cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfNDAxM2EzNjh9Cg==`

5. `$ echo 'cGljb0NURntrczNjcjM3NV80MW43X3M0ZjNfNDAxM2EzNjh9Cg==' | base64 -d`
    - Answer: `picoCTF{ks3cr375_41n7_s4f3_4013a368}`

## NOTES:
- `kubectl` drives a Kubernetes cluster; `-A` searches all namespaces (isolated "rooms"). Secrets live per-namespace.
- Kubernetes Secrets are stored Base64-encoded by default, **not** encrypted — as the flag itself states.
