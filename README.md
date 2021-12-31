# Trellis deploy GitHub Action

This action deploys [Trellis](https://github.com/roots/trellis) sites. It's a
composite action which takes care of the following automatically:

* Sets up Node JS using `actions/setup-node` including caching for npm/yarn in theme directory.
* Sets up Python (for Ansible) using `actions/setup-python`.
* Manages SSH keys and SSH agent forwarding so that GitHub can properly connect to your Trellis server.
* Sets your Ansible Vault password.
* Installs trellis-cli.
* Installs Trellis dependencies via `trellis init` (including caching).
* Deploys a Trellis site via `trellis deploy`.

## Example usage

```yaml
runs-on: ubuntu-latest
steps:
- uses: actions/checkout@v2
- uses: roots/trellis-deploy-action@v1
  with:
    ansible-vault-password: ${{ secrets.ANSIBLE_VAULT_PASSWORD }}
    ssh-private-key: ${{ secrets.TRELLIS_DEPLOY_SSH_PRIVATE_KEY }}
    ssh-known-hosts: ${{ secrets.SSH_KNOWN_HOSTS }}
    theme-name: roots
```

See the [example workflow](./example-workflow.yml) for a full example.

See [Workflow syntax for GitHub Actions](https://help.github.com/en/articles/workflow-syntax-for-github-actions) for more details on writing GitHub workflows.

## Inputs

## `ansible-vault-password`
**Required** Ansible Vault password. Use a [GitHub secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) for this value (example in usage
above).

## `ssh-private-key`
**Required** SSH private key (contents of key file). Use a [GitHub secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) for this value 
(example in usage above).

The secret value should look like this:

```plain
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACC7lG755UsWoanNiPs2+R3aPVwYkl/yXr3DoCN3+zVWMAAAAJjOjjZrzo42
awAAAAtzc2gtZWQyNTUxAAA7fnfDnd985UsWoanNiPs2+R3aPVwYkl/yXr3DoCN3+zVWMA
AAAEBxTBlN5ixcZOHFVGSnX2RXIp1Jg0lsR/GuzpF1p6ump7uUbvnlSxahqc2I+zb5Hdo9
XBiSX/JevcOgI3f7NVYwAAAADlDjd76fDFdfVwbG95AQIDBAUGBw==
-----END OPENSSH PRIVATE KEY-----
```

## `ssh-known-hosts`
**Required** Known hosts for SSH. Use a [GitHub secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets) for this value (example in usage
above).

This value is **not** just the hostname/IP, it needs be in the format that
`ssh-keyscan` returns (OpenSSH format):

```plain
github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
```

This value can also be found in your local `~/.ssh/known_hosts` file if you've
previously connected to the host.

## `theme-name`

**Required** The name of the WordPress theme; used for npm/cache caching.

## `deploy`

Whether to run the deploy or not. Default: `true`.

When set to `false`, the action will do all the same setup without actually
running the deploy.

## `deploy-site-name`

The name of the Trellis site to deploy. Default: main/first site name.

## `environment`

The name of the Trellis environment to deploy. Default: `production`.

## `node-version`

Version of Node JS to install and use. Default: `16`.

## `python-version`

Version of Python to install and use. Default: `3.9`.

## `trellis-cli-version`

Version of trellis-cli to install and use. Default: `latest`.
