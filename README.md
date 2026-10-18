# MiaoMenu-theme

[MiaoMenu](https://github.com/Yamada0001/MiaoMenu) 的官方主题仓库：存放四季主题图片与配套菜单模板，
由插件内置的 `/dgm download <主题ID>` 直接拉取安装。

## 目录结构

```
themes.json                       # 主题清单（插件入口）
themes/<主题ID>/java_menus/*.yml   # Java 版菜单模板
themes/<主题ID>/dialog_menus/*.yml # 原生 Dialog 菜单模板
themes/<主题ID>/pack/assets/...    # 资源包文件（贴图 + 物品模型）
```

## themes.json 格式（schema 1）

```json
{
  "schema": 1,
  "themes": [
    {
      "id": "spring_sakura",
      "name": "春日樱花",
      "description": "樱花飘落的春季主题，含粉樱树与花瓣粒子",
      "version": "1.0.0",
      "menus": { "java": "theme-spring", "dialog": "theme-spring" },
      "files": [
        {
          "source": "themes/spring_sakura/java_menus/theme-spring.yml",
          "target": "java_menus/theme-spring.yml",
          "sha256": "…"
        }
      ]
    }
  ]
}
```

字段说明：

| 字段 | 说明 |
|------|------|
| `id` | 主题 ID，只能包含小写字母、数字、`_`、`-`；也是 `image:` 使用的图片键 |
| `name` / `description` / `version` | 展示信息 |
| `menus.java` / `menus.dialog` | 安装后提示玩家打开哪个菜单 |
| `files[].source` | 本仓库内的相对路径 |
| `files[].target` | 安装到插件数据目录的相对路径 |
| `files[].sha256` | 可选，插件会校验下载结果 |

`target` 只允许落在这些目录，其它路径（含 `..`、绝对路径）会被插件直接拒绝：

```
java_menus/  dialog_menus/  bedrock_menus/  themes/  lang/
```

## 可用主题

### 四季主题

| ID | 名称 | 季节 | 图片键 |
|----|------|------|--------|
| `spring_sakura` | 春日樱花 | 春 | `miaomenu:menu/spring_sakura` |
| `summer_beach` | 夏日海滩 | 夏 | `miaomenu:menu/summer_beach` |
| `autumn_forest` | 秋日森林 | 秋 | `miaomenu:menu/autumn_forest` |
| `winter_snow` | 冬季飘雪 | 冬 | `miaomenu:menu/winter_snow` |

### 节日主题

| ID | 名称 | 菜单名 | 画面 |
|----|------|--------|------|
| `new_year` | 元旦 | `theme-newyear` | 雪夜广场、跨年钟楼、烟花与礼物 |
| `spring_festival` | 春节 | `theme-springfest` | 红灯笼、金色牌楼、舞龙、满地爆竹红纸 |
| `lantern_festival` | 元宵节 | `theme-lantern` | 河灯、石桥花灯、满月星空 |
| `qingming` | 清明节 | `theme-qingming` | 细雨石径、垂柳、青绿山野 |
| `dragon_boat` | 端午节 | `theme-dragonboat` | 龙舟竞渡、粽叶与竹蒸笼 |
| `qixi` | 七夕节 | `theme-qixi` | 月洞门、鹊桥、星河 |
| `mid_autumn` | 中秋节 | `theme-midautumn` | 满月、月饼茶席、桂花庭院 |
| `april_fools` | 愚人节 | `theme-aprilfools` | 彩带、恶作剧、倒转风车 |
| `national_day` | 国庆节 | `theme-nationalday` | 红旗花坛、城楼烟花 |
| `valentine` | 情人节 | `theme-valentine` | 玫瑰花园、心形气球、日落 |
| `halloween` | 万圣节 | `theme-halloween` | 南瓜灯、鬼屋、满月蝙蝠 |
| `christmas` | 圣诞节 | `theme-christmas` | 圣诞树、雪村木屋、极光 |
| `easter` | 复活节 | `theme-easter` | 彩蛋、兔子、春日草地与教堂 |

全部主题使用同一套 Minecraft 光影截图风格（体积光、环境光遮蔽、泛光、景深）。

## 安装方式

在服务器控制台或游戏内执行：

```
/dgm download                # 列出可用主题
/dgm download spring_sakura  # 安装
/dgm download winter_snow --force   # 覆盖已存在的同名菜单
```

安装后插件会自动热重载菜单。已存在的文件默认跳过，避免覆盖你手改过的配置。

## 资源包

主题图片位于 `themes/<主题ID>/pack/assets/`，安装后会落到服务器
`plugins/MiaoMenu/themes/assets/` 下。把该目录打包成资源包并托管到 HTTP 服务后，
在 `config.yml` 中配置：

```yaml
settings:
  resource-pack:
    enabled: true
    url: "https://你的域名/miaomenu-theme.zip"
    sha1: "资源包的 40 位 SHA-1"
    force: false
    prompt: "&d加载 MiaoMenu 主题资源包"
```

未下发资源包时，`image:` 物品会显示为普通纸张，菜单功能不受影响。

## 贡献主题

1. 在 `themes/<新ID>/` 下放 `java_menus/`、`dialog_menus/`、`pack/`；
2. 图片放在 `pack/assets/miaomenu/textures/gui/sprites/menu/<ID>.png`（建议 128×128 或 256×256 的整数倍方块风格）；
3. 物品模型放在 `pack/assets/miaomenu/models/menu/<ID>.json`；
4. 在 `themes.json` 的 `themes` 数组里追加一条记录，`sha256` 用 `sha256sum` 生成；
5. 提交 PR。

菜单里用 `image:<ID>` 引用主题图，例如：

```yaml
items:
  banner:
    slot: 4
    material: "image:spring_sakura"
    display_name: "&d&l春日樱花"
```
