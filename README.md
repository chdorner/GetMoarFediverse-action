The action has now moved into the main [GetMoarFediverse](https://github.com/g3rv4/GetMoarFediverse) repository.

# GetMoarFediverse-action

This action runs [GetMoarFediverse](https://github.com/g3rv4/GetMoarFediverse) as a GitHub action, which means... you don't need to host it anywhere!

## How to use it

There's a video [here](https://youtu.be/fyOSAzqpcQM), demoing all the steps needed for it to work. There's also a demo repo [here](https://github.com/g3rv4/GMFActionDemo).

1. Build a `config.json` as specified on the [GetMoarFediverse README](https://github.com/g3rv4/GetMoarFediverse/blob/main/README.md) and put it on your repo. Don't include `FakeRelayApiKey` on the json file.

It could be something like this:

```
{
    "FakeRelayUrl": "https://fakerelay.gervas.io",
    "Tags": [ "dotnet", "csharp" ],
    "Instances": [ "hachyderm.io", "mastodon.social" ]
}
```

One important thing is that you **do not** include the `FakeRelayApi`, as that would be visible by anyone.

2. Add a GitHub secret to your repository named `FAKERELAY_APIKEY` with the api key for the FakeRelay instance set up on `config.json`
3. Add a file named `.github/workflows/GetMoarFediverse.yml` with this content:

```
name: GetMoarFediverse

on:
  schedule:
    - cron: '*/15 * * * *'
  workflow_dispatch:

jobs:
  moar:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: chdorner/GetMoarFediverse-action@main
        with:
          config_file: ${{ github.workspace }}/config.json
          api_key: ${{ secrets.FAKERELAY_APIKEY }}
```

This will execute the action every 15 minutes automatically.

And that's it!
