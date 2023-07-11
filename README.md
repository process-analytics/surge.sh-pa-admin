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

The patch has been initiated and is updated by running `npx patch-package surge` to integrate local changes done in `node_modules/surge/lib` 

To benefit from the patches, run the following commands from the root of this repository:
- `npm install`
- then run `npx surge ...`


## Troubleshoot domains list errors

Because of [surge#307](https://github.com/sintaxi/surge/issues/307), the [surge-teardown-old-domains](./.github/workflows/surge-teardown-old-domains.yml) workflow may fail with the following error:
```
 /home/runner/.npm/_npx/23158936acd5c32d/node_modules/surge/lib/middleware/list.js:44
          project.timeAgoInWords.grey,
                                 ^
TypeError: Cannot read properties of undefined (reading 'grey')
    at /home/runner/.npm/_npx/23158936acd5c32d/node_modules/surge/lib/middleware/list.js:44:34
    at Array.forEach (<anonymous>)
```

The token may also be considered invalid by the `[surge-preview-tools](https://github.com/bonitasoft/actions/tree/main/packages/surge-preview-tools)` action for the same reason as it performs a `surge --list` to validate the token.

In this case, the patch can help
- run the "surge list" command locally or with https://github.com/process-analytics/surge.sh-pa-admin/actions/workflows/surge-list-domains.yml
- detect the domain with missing information. In the following example, the first domain has issue
```
Run npx surge --token *** list
   process-analytics-process-analytics-dev-site_preview-pr-1038.surge.sh                    N/A           N/A     N/A        Standard 
   1688564549686 process-analytics-process-analytics-dev-site_preview-pr-1013.surge.sh      6 days ago    surge   surge.sh   Standard 
   1688562584180 process-analytics-bpmn-visualization-js-demo_preview-pr-2748.surge.sh      6 days ago    surge   surge.sh   Standard 
```
- teardown the domains that cause issue
- relaunch the previously failing commands. They should succeed now.
