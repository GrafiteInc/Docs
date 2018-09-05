# Upgrade Guide

Regarding upgrades
-----
Grafite Builder is designed in a way that most upgrades will not affect your app. The Builder packages main job is to generate code in your web app, but it does come with the FormMaker and CrudMaker packages as well, which get updates more frequently. Once you start and app and build the parts you need, you can technically remove `grafite/builder` from your composer.json, but you would have to add in:

```
grafite/formmaker
grafite/crudmaker
grafite/crypto
```

## Compatibility

Due to the rebranding of Grafite Builder we had to draw a line, and cause breaking changes. The following is a general guide to upgrading, showing what is compatible and what is not.

| Package Version | Broken Compatibility |
|-----------------|-----------------|
| 2.5.x | None |
| 2.4.x | Any `yab` namespaced package |
| 1.0.x - 2.3.x | None |

## From Laracogs to Grafite Builder

!!! Warning
    This guide is partial there may be other parts that you extended or changed that are fundamentally broken and will require extensive work to refactor. We appologize for the inconvenience.

### Cosmetic
None, since components are added to your code base by the builder package.

### Everything Else
We dropped support for the following packages:

```
yab/laratest
yab/cerebrum
```

This means if you're trying to use a version prior to `2.4` you will get composer errors since the old packages were abandoned. Therefore, if you are stuck using Laravel 5.5 or older we recommend the following:

Remove:
```
yab/laracogs
```

Then require:
```
grafite/formmaker
grafite/crudmaker
grafite/crypto
```

Make sure you convert any namespace references as well:

`Yab\` to `Grafite\`

If you were using Cerebrum or Laratest, please contact (matt@grafite.ca)[matt@grafite.ca] to get a clone of those repos as they've now been archived and have since been removed from GitHub.
