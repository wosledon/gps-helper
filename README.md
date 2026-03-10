# GPS Helper 坐标转换工具库

一个轻量级的 C# 坐标转换工具库，提供 **WGS-84**、**GCJ-02**（高德地图）、**BD-09**（百度地图）三种主流坐标系统之间的相互转换功能。

## 功能特性

- 🗺️ **多坐标系支持**
  - WGS-84（GPS/全球定位系统采用）
  - GCJ-02（中国高德地图、腾讯地图等采用）
  - BD-09（中国百度地图采用）

- 🔄 **双向转换**
  - WGS-84 ↔ GCJ-02（GPS ↔ 高德）
  - GCJ-02 ↔ BD-09（高德 ↔ 百度）

- 🚀 **特性**
  - 中国地区范围检测
  - 高精度坐标变换算法
  - 零依赖，即插即用

## 使用方法

### 1. WGS-84 转 GCJ-02（GPS 转高德）
```csharp
double wgs84_lon = 120.155;
double wgs84_lat = 30.274;

double[] gcj02 = GpsHelper.WGS84_to_GCJ02(wgs84_lon, wgs84_lat);
Console.WriteLine($"高德经度: {gcj02[0]}, 高德纬度: {gcj02[1]}");
```

### 2. GCJ-02 转 WGS-84（高德转 GPS）
```csharp
double gd_lon = 120.161;
double gd_lat = 30.280;

double[] wgs84 = GpsHelper.GCJ02_to_WGS84(gd_lon, gd_lat);
Console.WriteLine($"GPS经度: {wgs84[0]}, GPS纬度: {wgs84[1]}");
```

### 3. GCJ-02 转 BD-09（高德转百度）
```csharp
double gd_lon = 120.161;
double gd_lat = 30.280;

double[] bd09 = GpsHelper.GCJ02_to_BD09(gd_lon, gd_lat);
Console.WriteLine($"百度经度: {bd09[0]}, 百度纬度: {bd09[1]}");
```

### 4. BD-09 转 GCJ-02（百度转高德）
```csharp
double bd_lon = 120.167;
double bd_lat = 30.286;

double[] gcj02 = GpsHelper.BD09_to_GCJ02(bd_lon, bd_lat);
Console.WriteLine($"高德经度: {gcj02[0]}, 高德纬度: {gcj02[1]}");
```

### 5. 直接转换（WGS-84 到 GCJ-02）
```csharp
double wgs84_lon = 120.155;
double wgs84_lat = 30.274;
double[] result = new double[2];

GpsHelper.Transform(wgs84_lon, wgs84_lat, result);
Console.WriteLine($"经度: {result[0]}, 纬度: {result[1]}");
```

## 公开 API

| 方法                          | 参数   | 返回值   | 说明                             |
| ----------------------------- | ------ | -------- | -------------------------------- |
| `WGS84_to_GCJ02(lon, lat)`    | double | double[] | GPS 坐标转高德坐标               |
| `GCJ02_to_WGS84(lon, lat)`    | double | double[] | 高德坐标转 GPS 坐标              |
| `GCJ02_to_BD09(lon, lat)`     | double | double[] | 高德坐标转百度坐标               |
| `BD09_to_GCJ02(lon, lat)`     | double | double[] | 百度坐标转高德坐标               |
| `Transform(lon, lat, result)` | double | void     | WGS-84 转 GCJ-02（结果存入数组） |

## 技术细节

### 坐标系统说明

- **WGS-84**：世界大地坐标系，GPS 原始坐标系统
- **GCJ-02**：中国国家标准，由国家测绘地理信息局制定（俗称"火星坐标"）
- **BD-09**：百度地图坐标系，在 GCJ-02 基础上二次加密

### 算法基础

使用克拉索天斯基椭球体（Krasovsky Ellipsoid）参数：
- 长半轴：6378245.0 米
- 第一偏心率：0.00669342162296594323

### 适用范围

- 仅对中国大陆地区进行坐标加密处理
- 中国区域外的坐标直接返回原值（不进行转换）
- 中国范围：[72.004°E, 137.8347°E] × [0.8293°N, 55.8271°N]

## 注意事项

⚠️ **重要**
- 返回值为 `double[2]` 数组：`[longitude, latitude]`（经度，纬度）
- 坐标系转换过程可能存在微小精度损失（通常小于 100 米）
- 不支持香港、澳门、台湾地区的坐标转换

## 许可证

MIT

## 相关资源

- [高德地图官方文档](https://lbs.amap.com/)
- [百度地图官方文档](https://lbsyun.baidu.com/)
- [GPS 坐标系统详解](https://en.wikipedia.org/wiki/Geographic_coordinate_system)
