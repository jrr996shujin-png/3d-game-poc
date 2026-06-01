# 森林追逐 — AI 生成 3D 游戏 POC

一个浏览器原生 3D 小游戏，**所有素材都是 AI 在线生成**（无第三方/社区素材），用 Three.js 拼装运行。

## 🎮 玩

**[👉 在线试玩 →](https://jrr996shujin-png.github.io/3d-game-poc/)**

桌面：`A`/`D` 换道 · `W` 冲刺 · `M` 静音
手机：左右滑动换道 · 上滑冲刺

跑到 200m 终点的金色宝箱 = 赢；狼追到 1.5m 内 = 输。

## 🛠️ 资产生成管线

| 资产 | 生成方式 | 平台 | 成本 |
|---|---|---|---|
| 草地贴图 (1024²) | Text-to-Image | Flux (via pollinations.ai) | $0 |
| 跑步参考图 | Text-to-Image | Flux | $0 |
| 主角（T-pose 老人） | Image-to-3D | Meshy 5 | $0.21 |
| **主角骨骼 + 跑步动画** | **Auto-Rigging** | **Meshy** | **$0.07** |
| 狼 / 宝箱 / 树 / 石 | Image-to-3D | Tripo v2.5 | 4 × $0.30 = $1.20 |
| 引擎集成 + 游戏代码 | LLM | Claude | $0 |
| BGM + 音效 | Web Audio 合成 | （浏览器内置） | $0 |
| **合计资产成本** | | | **~$1.48** |

## 🧠 技术栈

- **Three.js r165** + GLTFLoader + AnimationMixer（骨骼动画）
- **Meshy API** (Auto-Rigging 输出 `walking` + `running` 两段动画 GLB)
- **Tripo API** (image-to-3D, 含 PBR 贴图)
- **Flux** (Text-to-Image，pollinations.ai 免费代理)
- **Web Audio API** (无外部依赖的程序合成 BGM)

## 📁 项目结构

```
3d-game-poc/
├── index.html                          # 单 HTML 文件，~700 行
├── assets/
│   ├── villager_running_anim.glb       # Meshy 绑骨 + 动画 (7.0 MB)
│   ├── villager_running_ref.png        # Flux 生成的参考图（仅供参考）
│   ├── grass_tile.png                  # Flux 草地贴图 (128 KB)
│   └── tripo/
│       ├── wolf.glb                    # Tripo image-to-3D (11 MB)
│       ├── chest.glb                   # (15 MB)
│       ├── tree.glb                    # (16 MB)
│       └── rock.glb                    # (15 MB)
└── README.md
```

## 📊 结论（来自我们的横向评测）

**做带骨骼动画的 3D 游戏角色管线，Meshy 是三家里唯一一站式方案**：

- ✅ Meshy 有 Auto-Rigging + Animation API（友情价 $0.07/$0.04 一次）
- ❌ Tripo 没有
- ❌ Hi3D 没有（且只支持图生 3D，无文生）

详细的三家平台横向评测见 [3D 模型评测报告](#)（22 case × 3 平台 × 5 维度打分）。

## 🪪 资产授权

- AI 生成的资产：归生成者所有；Meshy 商用 license 见 [https://www.meshy.ai/policy](https://www.meshy.ai/policy)
- Tripo 商用 license 见 [https://www.tripo3d.ai/policy](https://www.tripo3d.ai/policy)
- 本仓库仅作技术 POC 演示，不构成对资产商用合规性的承诺

## 📅 时间线

- 2026-05-18：完成 22 case × 3 平台 image-to-3D 横向评测
- 2026-05-19：补 text-to-3D 评测（Hi3D 不支持除外）
- 2026-06-01：搭出此 POC 游戏 + 上线 GitHub Pages
