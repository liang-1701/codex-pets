# Codex Pets

一组可直接安装的自定义宠物包，遵循 [Codex Pet 包格式](http://codexpet.xyz/zh/spec) 的 **V2 精灵图集**规范（1536×2288，8 列 × 11 行）。

## 宠物一览

| ID | 显示名 | 形象 | 图集尺寸 | 包体积 |
| --- | --- | --- | --- | --- |
| `dragon` | Dragon | 圆滚滚的 Q 版紫色幼龙，金色鹿角、蓝粉相间的蓬松鬃毛，自带一朵与身体相连的云 | 1536×2288 | 3.1 MB |
| `husky` | Husky | 灰白蓬松的哈士奇幼犬，冰蓝色眼睛、立耳，吐着粉色舌头 | 1536×2288 | 2.3 MB |
| `mochi` | Mochi | 奶油白色的 Q 版小猫，带灰色虎斑块，琥珀色大眼睛 | 1536×2288 | 1.4 MB |
| `tiger` | Tiger | 好奇的橘色小老虎，粗黑条纹、毛茸茸，圆圆亮眼 | 1536×2288 | 3.2 MB |

## 安装

每个子文件夹就是一个完整宠物包，把它复制到 Codex 的宠物目录即可：

```
${CODEX_HOME:-$HOME/.codex}/pets/<id>/
├── pet.json
└── spritesheet.webp
```

**macOS / Linux**

```bash
# 以 dragon 为例
CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
mkdir -p "$CODEX_HOME/pets"
cp -r dragon "$CODEX_HOME/pets/dragon"
```

**Windows (PowerShell)**

```powershell
# 以 dragon 为例
$CodexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { "$env:USERPROFILE\.codex" }
New-Item -ItemType Directory -Force -Path "$CodexHome\pets" | Out-Null
Copy-Item -Recurse -Force .\dragon "$CodexHome\pets\dragon"
```

重启 Codex 后即可在宠物列表中看到。四个全部安装：

```bash
for p in dragon husky mochi tiger; do cp -r "$p" "$CODEX_HOME/pets/$p"; done
```

## 仓库结构

```
codex-pets/
├── LICENSE                 # MIT
├── README.md
├── dragon/
│   ├── pet.json
│   └── spritesheet.webp
├── husky/
│   ├── pet.json
│   └── spritesheet.webp
├── mochi/
│   ├── pet.json
│   └── spritesheet.webp
└── tiger/
    ├── pet.json
    └── spritesheet.webp
```

## 包格式说明

### pet.json

```json
{
  "id": "dragon",
  "displayName": "Dragon",
  "description": "A chubby chibi purple baby dragon with golden antlers, a fluffy blue-pink mane, and its own fluffy cloud that is part of its body.",
  "spriteVersionNumber": 2,
  "spritesheetPath": "spritesheet.webp"
}
```

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `id` | 是 | 宠物唯一标识，建议与文件夹名一致 |
| `displayName` | 是 | 界面展示名称 |
| `description` | 是 | 一句话形象描述（英文，供客户端使用） |
| `spriteVersionNumber` | **是** | 必须为 `2`。缺失时客户端按 V1（9 行、1536×1872）解析，会导致 2288 高的图集被拒绝 |
| `spritesheetPath` | 是 | 精灵图相对路径，通常为 `spritesheet.webp` |

### spritesheet.webp

- 尺寸：**1536 × 2288**（V2 硬性要求）
- 网格：8 列 × 11 行，单格 **192 × 208**
- 格式：透明背景的 WebP 或 PNG（RGBA）
- 无网格线、标签、帧编号；每个动作行至少有 1 个非空帧，播放时从第 0 列起连续取帧，尾部空帧忽略

| 行 | 状态 | 建议帧数 |
| --- | --- | --- |
| 0 | idle 待机 | 6 |
| 1 | running-right 向右跑 | 8 |
| 2 | running-left 向左跑 | 8 |
| 3 | waving 挥手 | 4 |
| 4 | jumping 跳跃 | 5 |
| 5 | failed 失败 | 8 |
| 6 | waiting 等待 | 6 |
| 7 | running 跑动 | 6 |
| 8 | review 检查 | 6 |
| 9 | look-directions-a | 8 |
| 10 | look-directions-b | 8 |

行 9–10 为 V2 新增的 16 个朝向：行 9 覆盖 000°–157.5°，行 10 覆盖 180°–337.5°，每格间隔 22.5°；`000` 表示正上方（12 点方向），而非正面——正面属于无向量死区，回退到 idle。

## 添加新宠物

1. 新建文件夹，名称即 `id`
2. 放入 `pet.json`（记得 `spriteVersionNumber: 2`）与 1536×2288 的透明 `spritesheet.webp`
3. 校验：图中无网格线/文字，每行至少一个非空帧，形象在各行之间保持一致

## License

[MIT](LICENSE) © 2026 liang-1701

宠物形象与美术素材由本仓库作者提供，使用请遵循上述许可。
