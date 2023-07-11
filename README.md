# surge.sh-pa-admin

Manage the surge.sh domains of the Process Analytics project.

## Available workflows

Workflows are used to manage surge.sh domains specific to the Process Analytics project.

- [List all domains](.github/workflows/surge-list-domains.yml)
- [Teardown a domain](.github/workflows/surge-teardown-domain.yml)
- [Teardown old domains](.github/workflows/surge-teardown-old-domains.yml)


## Patching the "surge" CLI

`surge` is patched using [patch-package](https://www.npmjs.com/package/patch-package) to integrate bug fixes not available in a released version:
- https://github.com/sintaxi/surge/issues/307 "Surge List not working" implementation from https://github.com/sintaxi/surge/pull/319 (opened in 2018-04-10)

The patch has been created by modifying manually `node_modules/mxgraph/javascript/dist/build.js`

The patch has been initiated and is updated by running `npx patch-package surge` to integrate local changes done in `node_modules/surge/lib` 
