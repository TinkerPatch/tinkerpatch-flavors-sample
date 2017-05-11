# tinkerpatch-easy-sample

[![Build Status](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample.svg?branch=master)](https://travis-ci.org/TinkerPatch/tinkerpatch-flavors-sample)
[ ![Download](https://api.bintray.com/packages/simsun/maven/tinkerpatch-android-sdk/images/download.svg) ](https://bintray.com/simsun/maven/tinkerpatch-android-sdk/_latestVersion)
[![Join Slack](https://slack.tinkerpatch.com/badge.svg)](https://slack.tinkerpatch.com)

## 多Flavors的Sample

在TinkerPatchSupport中添加如下字段, 如果你只是多渠道的需求，建议不要使用Flavor。多flavor必须在后台建立相应的基线工程(如下例子的命名规则为：appVersion_flavorName)，每次生成补丁时也必须对应的生成多个分别上传。

这里增加了`tinkerPatchAllFlavorsDebug` 和 `tinkerPatchAllFlavorsRelease` 用于一次性生成所有flavors的Patch包。

**如果只是多渠道的需求，建议不要使用flavor的方式。首先其打包很慢，其次需要维护多个基线包，后期维护成本也很大。Tinker官方推荐 [packer-ng-plugin](https://github.com/mcxiaoke/packer-ng-plugin )或者 [walle](https://github.com/Meituan-Dianping/walle) 来进行多渠道打包，其中walle是支持最新的SchemaV2签名的。**

在tinkerpatchSupport增加如下字段：

```gradle
    productFlavors {
        flavor {
            flavorName = "flavor1"
            // 后台需要按照每个flavor的appVersion来建立独立的工程，并单独下发补丁
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}${variantName}/"
            name = "${project.name}-${flavorName}${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"
        }

        flavor {
            flavorName = "flavor2"
            // 后台需要按照每个flavor的appVersion来建立独立的工程，并单独下发补丁
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}${variantName}/"
            name = "${project.name}-${flavorName}${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"
        }
    }
```

## 增加加固flavor的sample

这里默认大家有同时生成加固渠道与非加固渠道的需求，如果只是单一需要加固，可以直接在配置中开启`protectedApp = true`即可。

可以参考tinkerpatch.gradle文件：
```gradle
    productFlavors {
        flavor {
            flavorName = "protect"
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}-${variantName}/"
            name = "${project.name}-${flavorName}-${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"

            /** 开启加固开关，上传此flavor的apk到加固网站进行加固 **/
            protectedApp = true
        }

        flavor {
            flavorName = "flavor1"
            appVersion = "${tinkerpatchSupport.appVersion}_${flavorName}"

            pathPrefix = "${bakPath}/${baseInfo}/${flavorName}-${variantName}/"
            name = "${project.name}-${flavorName}-${variantName}"

            baseApkFile = "${pathPrefix}/${name}.apk"
            baseProguardMappingFile = "${pathPrefix}/${name}-mapping.txt"
            baseResourceRFile = "${pathPrefix}/${name}-R.txt"
        }
    }
```

### 加固步骤：

1. 生成开启`protectedApp = true`的基础包（这里假设此APK名为：`protected.apk`）  
2. 上传`protected.apk`到相应的加固网站进行加固，并发布应用市场（请遵循各个加固网站步骤，一般为下载加固包-》重新签名-》发布重签名加固包）  
3. 在tinkerPatch后台根据appVersion建立相应的App版本（比如这里2个flavor，就需要建立2个App版本。App版本即为各自flavor中配置的appVersion） 
4. 生成各个flavor的patch包，并上传至相应的App版本中。

**protectedApp=true, 这种模式仅仅可以使用在加固应用中**

### 支持列表:

| 加固厂商 | 测试      | 
| --------| --------- | 
| 乐加固   | Tested  | 
| 爱加密   |  Tested |
| 梆梆加固 | Tested  |
| 360加固 | Tested，2017.5.8之后的[版本](http://jiagu.360.cn/protection?s=1) | 
| 其他    | 请自行测试，只要满足下面规则的都可以支持 | 


这里是否支持加固，需要加固厂商明确以下两点：

1. 不能提前导入类；
2. 在art平台若要编译oat文件，需要将内联取消。


[相关文档](http://tinkerpatch.com/Docs/SDK.md)
