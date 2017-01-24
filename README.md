# tinkerpatch-easy-sample

[![Build Status](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample.svg?branch=master)](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample)
[ ![Download](https://api.bintray.com/packages/simsun/maven/tinkerpatch-android-sdk/images/download.svg) ](https://bintray.com/simsun/maven/tinkerpatch-android-sdk/_latestVersion)
[![Join Slack](https://slack.tinkerpatch.com/badge.svg)](https://slack.tinkerpatch.com)

多Flavors的Sample

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
