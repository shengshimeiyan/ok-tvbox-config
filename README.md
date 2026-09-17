# OK影视 2.5.0 逐源兼容性测试套件

## 目标
找出 OK影视 2.5.0 精简版内置 spider 到底认识哪些 `csp_*` 名字，避免整个接口导入闪退。

## 测试策略
**每次只导入一个 JSON**，每次测试一个 csp_ 名字。

- **✅ 能加载出源**：这个 csp_ 名字支持，安全
- **❌ 应用闪退**：这个 csp_ 名字不支持，删掉它
- **⚠️ 源显示但内容空**：spider 认得但后端挂了

## 文件列表

| 文件 | 测试的 csp_ |
|------|------------|
| `00-sentinel.json` | `csp_Douban`（哨兵，几乎必支持） |
| `01-jpys-jinpai.json` | `csp_Jpys` |
| `02-bili.json` | `csp_Bili` |
| `03-gz360.json` | `csp_Gz360` |
| `04-sp360.json` | `csp_SP360` |
| `05-auete.json` | `csp_Auete` |
| `06-kuaikan.json` | `csp_Kuaikan` |
| `07-freeok.json` | `csp_Freeok` |
| `08-nmys.json` | `csp_Nmys` |
| `09-vidhub2.json` | `csp_Vidhub2` |
| `10-xbpq.json` | `csp_XBPQ` |
| `11-wogw.json` | `csp_Wogg` |
| `12-lvdou.json` | `csp_Lvdou` |

## 使用步骤

### 1. 创建 GitHub 仓库
- 新建一个 **public** 仓库（jsDelivr 只缓存公开仓库）
- 把 `tvbox-test-suite/` 里的所有 `.json` 文件放上去
- 建议目录：`config/` 或根目录

### 2. 首次推送后等 1-5 分钟
jsDelivr 的缓存需要时间预热。第一次访问某个 URL 时会自动触发。

### 3. 逐个测试
导入顺序：**从 00-sentinel 开始**，成功后再往下。

jsDelivr URL 模板：
```
https://cdn.jsdelivr.net/gh/<你的用户名>/<仓库名>@main/<文件名>.json
```

**示例**（假设你用户名 `alice`，仓库 `tvbox`）：
```
https://cdn.jsdelivr.net/gh/alice/tvbox@main/00-sentinel.json
https://cdn.jsdelivr.net/gh/alice/tvbox@main/01-jpys-jinpai.json
https://cdn.jsdelivr.net/gh/alice/tvbox@main/02-bili.json
...
```

**每次测试完**：
1. 记下结果（能加载 / 闪退 / 源空）
2. **清空之前的配置**（在 OK影视 设置 → 数据源 → 重置/清空）
3. 导入下一个

### 4. 收集结果
把每个文件的测试结果发我，我帮你合并成一个可用的完整配置。

## 如果 00-sentinel 就闪退

说明 OK影视 2.5.0 精简版的 **spider 模块本身被删了**——外部 spider URL 完全无法加载。这时候任何 `csp_*` 都没救，只能找 OK猫作者要官方兼容配置。

## 备用方案：不用 GitHub，用临时托管

如果不想建仓库，可以用：
- **Codeberg**（类似 GitHub，也支持 jsDelivr 的 gh/ 前缀？需确认）
- **临时文件服务**：把 JSON 放任意 HTTP 静态服务上，盒子直接导入
  ```
  # 手机热点方案
  python3 -m http.server 8000
  # 盒子导入 http://你的手机IP:8000/00-sentinel.json
  ```

## 关于夸克网盘里的接口

夸克网盘是 JS 渲染，我这边抓不到内容。**请把里面具体的 JSON 内容复制给我**，我帮你判断每个源在 OK影视 2.5.0 上是否有闪退风险。

或者告诉我网盘里每个文件的**导出 URL**（如果能转成直接 HTTP 链接的话），我可以先测后端是否活着。
