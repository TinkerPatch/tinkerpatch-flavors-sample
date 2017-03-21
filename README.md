# tinkerpatch-easy-sample

[![Build Status](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample.svg?branch=master)](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample)
[ ![Download](https://api.bintray.com/packages/simsun/maven/tinkerpatch-android-sdk/images/download.svg) ](https://bintray.com/simsun/maven/tinkerpatch-android-sdk/_latestVersion)
[![Join Slack](https://slack.tinkerpatch.com/badge.svg)](https://slack.tinkerpatch.com)

多Flavors的Sample

----

在TinkerPatchSupport中添加如下字段, 如果你只是多渠道的需求，建议不要使用Flavor。多flavor必须在后台建立相应的基线工程(命名规则为：appVersion_flavorName)，每次生成补丁时也必须对应的生成多个分别上传。

这里增加了`tinkerPatchAllFlavorsDebug` 和 `tinkerPatchAllFlavorsRelease` 用于一次性生成所有flavors的Patch包。

**如果只是多渠道的需求，建议不要使用flavor的方式。首先其打包很慢，其次需要维护多个基线包，后期维护成本也很大。Tinker官方推荐 [packer-ng-plugin](https://github.com/mcxiaoke/packer-ng-plugin )或者 [walle](https://github.com/Meituan-Dianping/walle) 来进行多渠道打包，其中walle是支持最新的SchemaV2签名的。**

在tinkerpatchSupport增加如下字段：

```gradle
    productFlavors {
        flavor {
            flavorName = "flavor1"
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}${variantName}/"
            name = "${project.name}-${flavorName}${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"
        }

        flavor {
            flavorName = "flavor2"
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}${variantName}/"
            name = "${project.name}-${flavorName}${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"
        }
    }
```

[相关文档](http://tinkerpatch.com/Docs/SDK.md)
