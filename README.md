# Taxtile

Claude Code plugin for Irish tax: income tax, USC, PRSI, CGT, credits and take-home pay, as tools plus a guidance skill. Web calculator: https://taxtile.surge.sh

```
/plugin marketplace add AranBSmith/taxtile
/plugin install taxtile@taxtile-marketplace
```

See [plugin/README.md](plugin/README.md) for usage and details. Not financial advice.

## Licence

MIT, see [LICENSE](LICENSE).

## Releases

Versions are managed by [Release Please](https://github.com/googleapis/release-please) from [Conventional Commits](https://www.conventionalcommits.org): `fix:` bumps the patch, `feat:` the minor, `feat!:` or a `BREAKING CHANGE:` footer the major (minor while below 1.0). Merging the release PR bumps `plugin/.claude-plugin/plugin.json`, updates `CHANGELOG.md` and tags the release. Installed users only get updates on a version bump.
