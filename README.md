Checking if Cypress automatically handles _all_ TypeScript files: specs, plugins, support

```
npm install
```

## Found

- need to set `"pluginsFile": "cypress/plugins/index.ts"` to load TypeScript plugins file. Default value does not load the plugins file at all.

The plugins file prints the filename.

Default `cypress.json` does not print the console log message, meaning the plugins file is silently skipped

```
$ g st
## master
A  cypress.json
A  cypress/integration/spec.ts
A  cypress/plugins/index.ts
A  cypress/support/index.ts
M  package-lock.json
M  package.json
~/git/test-ts-plugins on master*
$ npx cypress run

====================================================================================================

  (Run Starting)

  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ Cypress:    4.6.0                                                                              │
  │ Browser:    Electron 80 (headless)                                                             │
  │ Specs:      1 found (spec.ts)                                                                  │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘


────────────────────────────────────────────────────────────────────────────────────────────────────

  Running:  spec.ts                                                                         (1 of 1)


  ✓ works (38ms)

  1 passing (55ms)


  (Results)

  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ Tests:        1                                                                                │
  │ Passing:      1                                                                                │
  │ Failing:      0                                                                                │
  │ Pending:      0                                                                                │
  │ Skipped:      0                                                                                │
  │ Screenshots:  0                                                                                │
  │ Video:        true                                                                             │
  │ Duration:     0 seconds                                                                        │
  │ Spec Ran:     spec.ts                                                                          │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘


  (Video)

  -  Started processing:  Compressing to 32 CRF
  -  Finished processing: /Users/gleb/git/test-ts-plugins/cypress/videos/spec.ts.mp4     (0 seconds)


====================================================================================================

  (Run Finished)


       Spec                                              Tests  Passing  Failing  Pending  Skipped
  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ ✔  spec.ts                                   54ms        1        1        -        -        - │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘
    ✔  All specs passed!                         54ms        1        1        -        -        -

~/git/test-ts-plugins on master*
```

Set plugins file to `cypress/plugins/index.ts` and see the message

```
$ npx cypress run
in /Users/gleb/git/test-ts-plugins/cypress/plugins/index.ts

====================================================================================================

  (Run Starting)

  ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ Cypress:    4.6.0                                                                              │
  │ Browser:    Electron 80 (headless)                                                             │
  │ Specs:      1 found (spec.ts)                                                                  │
  └────────────────────────────────────────────────────────────────────────────────────────────────┘


────────────────────────────────────────────────────────────────────────────────────────────────────

  Running:  spec.ts                                                                         (1 of 1)


  ✓ works (56ms)

```


## Support file

Same with support file - to have it load needed to set it explicitly in the `cypress.json` file

```json
{
  "fixturesFolder": false,
  "pluginsFile": "cypress/plugins/index.ts",
  "supportFile": "cypress/support/index.ts"
}
```

## Debugging

Run with `DEBUG=cypress:server:config` to see verbose messages

```
cypress:server:config setting support file /Users/gleb/git/test-ts-plugins/cypress/support +1ms
cypress:server:config for project root /Users/gleb/git/test-ts-plugins +0ms
cypress:server:config support file /Users/gleb/git/test-ts-plugins/cypress/support does not exist +4ms
cypress:server:config support file is default, check if /Users/gleb/git/test-ts-plugins/cypress exists +0ms
cypress:server:config support folder exists, set supportFile to false +0ms
cypress:server:config setting plugins file /Users/gleb/git/test-ts-plugins/cypress/plugins +1ms
cypress:server:config for project root /Users/gleb/git/test-ts-plugins +0ms
cypress:server:config plugins file does not exist +1ms
cypress:server:config plugins file is default, check if /Users/gleb/git/test-ts-plugins/cypress exists +0ms
cypress:server:config plugins folder exists, set pluginsFile to false +0ms
cypress:server:config set scaffold paths +0ms
```
