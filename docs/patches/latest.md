latest
========

**breaking changes**
- all unboxed `zeus` boxes will need to be re-unboxed to support the new `zeus_boxes` architecture

### [docs](https://docs.liquidapps.io/en/stable/)
- add CoVax chain section for becoming BP or DSP
- add example chains section of LiquidX chains
- add [EOS Block Producer Benchmarks](https://www.alohaeos.com/tools/benchmarks#networkId=21&timeframeId=2) | courtesy of [Aloha EOS](https://www.alohaeos.com/)
- use eos 2.0.5
- update documentation to support `zeus_boxes` refactor
- add `chain-threads=8` `eos-vm-oc-compile-threads=2` to nodeos example `config.ini` in docs
- add [LiquidStorage section](../developers/storage-getting-started)
- update [LiquidAccounts section](../developers/vaccounts-getting-started)
- update [LiquidVRAM section](../developers/vram-getting-started)

### [@liquidapps/zeus-cmd](https://www.npmjs.com/package/@liquidapps/zeus-cmd)
- use 8887 instead of 8889 for state history port to match DSP docs
- skip `01-dapp-client.js` if built
- add price feed example, price feed uses LiquidHarmony's oracles and LiquidScheduler's cron to fetch a price periodically and only use CPU when the price has changed from the last recorded price by more or less than 1%
- add `'--eos-vm-oc-compile-threads=4'` and `--chain-threads=4` to local nodeos
- update `zeus` to modularize logic into `zeus_boxes` directory making `zeus` more like `npm`, to create or unbox a new box start with `zeus box create [name]` then if you wish to unbox and existing box `zeus unbox <BOX>`
- zeus now offers versioning of boxes
    - zeus now offers optional `zeus unbox <BOX>@[VERSION]`
    - can add and remove boxes with `zeus box add <BOX> [VERSION] [URI]` `zeus box remove <BOX> [VERSION]`
    - to update an existing box, run `zeus unbox <BOX>@[VERSION]`, if no version specified, latest used, will unbox everything again with new version
    - to only add new boxes, unbox after update with `--no-update`
- fixes
    - if Mac, detect and skip `--eos-vm-oc-enable` flags as they are not supported

### [@liquidapps/dsp](https://www.npmjs.com/package/@liquidapps/dsp)
- added warning to ensure `trace-history = true` set in nodeos `config.ini`
- add how many blocks behind the head block in demux heartbeat
- add `DATABASE_TIMEOUT` to sample toml, adjust time before database connection times out
- add `DEMUX_PROCESS_BLOCK_CHECKPOINT` to sample toml, amount of blocks to pass before updating database with last processed block
- add `disabledServices` to ecosystem file to prevent pre-alpha services from being setup
- add `DEMUX_MAX_MEMORY_MB` option to ecosystem file to set maximum amount of memory that can be used by demux
- add `dsp_account_permissions` option for each sidechain
- fixes
    - handle `TypeError: Cannot read property 'this_block' of undefined` in demux

### [LiquidAccount Service](https://docs.liquidapps.io/en/v2.0/services/vaccounts-service.html)
- add cross chain support for LiquidAccounts using LiquidX

### [LiquidVRAM Service](https://docs.liquidapps.io/en/stable/services/ipfs-service.html)
- add cross chain reading of vRAM table data

### [LiquidStorage Service](https://docs.liquidapps.io/en/stable/services/storage-service.html)
- add dapp-client examples for `get_uri.ts`, `unpin_public_file.ts`, `upload_archive_to_liquidstorage.ts`, `upload_file_to_liquidstorage.ts`, `upload_public_file_from_vaccount.ts`
- add sidechain storage unit test
- add `zeus storage upload <ACCOUNT_NAME> package.json <ACCOUNT_PRIVATE_KEY>` and `zeus storage unpin <ACCOUNT_NAME> <IPFS_URI_RETURNED_ABOVE> <ACCOUNT_PRIVATE_KEY>` zeus commands

### [LiquidHarmony Service](https://docs.liquidapps.io/en/stable/developers/harmony-getting-started.html)
- add check to ensure each DSP only returns one oracle response
- add hook to assert on oracle fetch before geturi is fired
- add `Oracle minimum threshold check` unit test
- reduce oracle retries to 10 from 100
- add `shouldAbort` `eosio::check` handler for aborting oracle service request
- add `echo` oracle, which uses the same structure as `http` or `https` however the uri is replaced with a desired return value. This return value must be a base64 encoded string. 
    - `echo`: Mimics a GET request that returns text
    - `echo+json`: Mimics a GET request that returns JSON
    - `echo+post`: Mimics a POST request that returns text
    - `echo+post+json`: Mimics a POST request that returns JSON 

### [LiquidScheduler Service](https://docs.liquidapps.io/en/stable/developers/cron-getting-started.html)
- run exponential backoff forever, was 10 retries max
- add `shouldAbort` `eosio::check` handler for rescheduling cron without CPU

### [@liquidapps/dapp-client](https://www.npmjs.com/package/@liquidapps/dapp-client)
- patch new secondary index RPC API support
- updated Dapp Client to support cross chain Liquid Accounts