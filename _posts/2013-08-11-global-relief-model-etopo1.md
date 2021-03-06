---
title: 全球地形起伏模型 ETOPO1
date: 2013-08-11
author: SeisMan
categories: 地球物理相关资源
tags: [GMT, 地形, 数据, 网格, 高程]
---

ETOPO1 是分辨率为 1 弧分的全球地形起伏模型，其包含了陆地地形和海洋水深的数据，是目前已知的分辨率
最高的地形起伏数据。

官方主页位于： <http://www.ngdc.noaa.gov/mgg/global/>

<!--more-->

其分为两个版本，Ice Surface 和 Bedrock，两个版本基本一致。不同之处在于在处理南极洲和
Greenland 地形时，前者给出的是加上冰盖层之后的高程，后者给出的是岩床的高程。

对于每个版本又细分为 grid-registered 和 cell-registered，其中 grid-registered 是权
威版本，cell-registered 是衍生版本，因而**推荐下载使用grid-registered版本**。

在每个子版本下又有多种数据格式，netCDF，binary，xyz，tiff。

## 下载

本文使用的是**grid-registered版本**的**binary格式**的数据文件：
[etopo1\_ice\_g\_i2.zip](http://www.ngdc.noaa.gov/mgg/global/relief/ETOPO1/data/ice_surface/grid_registered/binary/etopo1_ice_g_i2.zip)。

下面的说明也只针对 binary 格式有效。

## 解压

    7za e etopo1_ice_g_i2.zip

解压之后的 `.bin` 文件为二进制网格文件， `.hdr` 文件为头段文件

## 拷贝

    sudo cp etopo1_ice_g_i2.bin /usr/local/GMT-4.5.12/share/dbase

## 修改 grdraster.info

直接将下面的语句复制到 `grdraster.info` 中即可。关于语句为什么要这么写，需要参考 hdr 文件的内容。

    9 "ETOPO1 Ice Surface"          "m"     -R-180/180/-90/90       -I1m            GG i 1          0       -32768  etopo1_ice_g_i2.bin     L

同理，对于 bedrock 版本的网格数据，其 `grdraster.info` 为:

    10 "ETOPO1 Bedrock"             "m"     -R-180/180/-90/90       -I1m            GG i 1          0       -32768  etopo1_bed_g_i2.bin     L

## 备注

如果下载的是 netCDF 格式的网格文件，需要利用如下命令将数据转换为 binary 格式:

    grdreformat ETOPO1_Ice_g_gmt4.grd etopo1_ice_g_i2.bin=bs -N -V

当然对于 netCDF 格式的网格文件，也可以直接用 `grdcut` 做裁剪，用 `grdimage` 画图，省去了
`grdraster` 的步骤。哪个方便用哪个。
