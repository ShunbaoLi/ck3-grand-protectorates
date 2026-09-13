# 天子敕建大都护府 (CK3 Grand Protectorates)

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-ck3--grand--protectorates-181717.svg?logo=github)](https://github.com/ShunbaoLi/ck3-grand-protectorates)
[![Changelog](https://img.shields.io/badge/Changelog-Keep_a_Changelog-blueviolet.svg)](CHANGELOG.md)
[![CK3 Version](https://img.shields.io/badge/CK3_Version-1.18%20%7C%201.19+-orange.svg)](https://ck3.paradoxwikis.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[简体中文](README.md) | [English](README.en.md)**

</div>

---

## 📌 项目简介

**天子敕建大都护府 (CK3 Grand Protectorates)** 是一款专为《十字军之王3》（Crusader Kings III）天朝霸权（Tier 6 Hegemony）打造的高质量、深度解耦的边疆行政与法理重构功能 Mod。

在原版与各大扩展（如《东亚与帝国》TGP）中，天朝面对辽阔的四裔封疆，往往缺乏符合历史大唐气象的边防总督行政机构；而传统的法理漂移耗费百年，无法动态根据战局与开拓进程灵活划设边疆管辖区。

本 Mod 为天朝天子赋予至高无上的**“天子开府”**与**“更勘疆理”**皇权敕令：
- 独立新建专属于天朝霸权的**四大帝国级（Tier 5）大都护府**（安西、安东、安北、安南）；
- 融合原生规范组件的**独立专属多选管理面板**，支持通过 Checkbox 自由勾选领内王国划入大都护府帝国法理；
- 具备严格的**原始母帝国变量记录与退回闭环**，以及**清空全选自动安全裁撤**机制；
- 深度适配天朝官制，自动绑定**都护行省契约（celestial_province_protectorate）**与**总督委任法（Governor Succession）**，官称即时生效为“大都护”，机构为“大都护府”。

> 📜 **版本更新历史**：详细记录请查阅 [CHANGELOG.md](CHANGELOG.md)。

---

## 🏛️ 四大都护府规划矩阵 (The Four Grand Protectorates)

为确保与游戏原版及历史剧本完全解耦，本 Mod 采用纯独立头衔设计（安东大都护府独立定义，绝不复用或覆写原版泛辽东 e_andong，确保契丹/辽剧本 100% 零冲突）：

| 大都护府 | 头衔代码 | 历史治所 | 统摄地理方向 | 官制与政体 |
| :--- | :--- | :--- | :--- | :--- |
| **安西大都护府** | e_anxi_protectorate | 西州 (c_xizhou) | 西域、河中、中亚绿洲、波斯、吐蕃诸邦 | 天朝都护行省契约 · 总督委任法 |
| **安东大都护府** | e_andong_protectorate | 幽州 (c_youzhou) | 辽东、朝鲜半岛诸国、扶桑群岛 | 天朝都护行省契约 · 总督委任法 |
| **安北大都护府** | e_anbei_protectorate | 五原 (c_wuyuan) | 蒙古大漠草原、贝加尔、西伯利亚冻原诸部 | 天朝都护行省契约 · 总督委任法 |
| **安南大都护府** | e_annan_protectorate | 交州 / 龙编 (c_thang_long) | 岭南外藩、交趾、中南半岛、南洋诸邦 | 天朝都护行省契约 · 总督委任法 |

---

## ⚙️ 行政架构与法理闭环流程 (Architecture & Logic Loop)

`mermaid
flowchart TD
    A[天朝皇帝天子] -->|点击决议| B(天子开府：设大都护府)
    B --> C{选择设府方向}
    C -->|安西 / 安东 / 安北 / 安南| D[呼出专属管辖多选面板]
    
    D --> E[勾选候选王国]
    E -->|记录 original_de_jure_empire| F[执行 set_de_jure_liege_title 划入大都护府法理]
    
    D --> G[取消勾选既有王国]
    G -->|读取 original_de_jure_empire| H[安全退回原本母帝国法理]
    
    D --> I[全部取消勾选 勾选数=0]
    I -->|触发裁撤警示| J[执行 destroy_title 销毁大都护府并返还威望]
`

### 1. 决议【天子开府：设大都护府】
- **触发门槛**：必须是玩家（is_ai = no），拥有天朝霸权（	ier_hegemony），且领地内控制至少 1 个符合设府地理区域的非中原王国。
- **开府流程**：弹出选择引导事件，选定方向后即刻激活帝国级头衔，赋予总督委任法，并将选定王国正式编入大都护府法理。

### 2. 决议【更勘辖区：调整大都护府法理】
- 当某一前置大都护府建立后，开放对应都护府专属更勘决议。
- **已归属王国**：展示为默认勾选状态 [√]；
- **领内候选王国**：展示为未勾选状态 [ ]；
- **精准退回与闭环**：取消勾选的王国依据变量精准退回母帝国法理，即使原母帝国已被销毁或休眠也能安全归位；
- **裁撤机制**：全清勾选确认后自动销毁头衔并返还 300 威望；
- **人事解耦**：Mod 专注法理统辖，封臣实际效忠自然协同原版“可转封封臣给法理领主”逻辑，不强制捆绑。

---

## 🖥️ 专属界面规范 (UI & UX Design)

专属管理面板（gui/window_grand_protectorate.gui）严格遵循原版高质量组件设计标准：
1. **复选框控件**：复用原版大型工程标准规范的 utton_checkbox 组件；
2. **头衔卡片**：参考原版分封界面，包含金冠小盾徽（coa_title_tiny_crown）、王国名称、原母帝国名称及公国数量副标；
3. **裁撤动态警示**：当勾选列表为空时，动态显现醒目警告：“⚠️ 辖区已清空，该大都护府将被正式裁撤”；
4. **底部大按钮**：醒目金色大按钮 【颁布天子敕令：更勘疆理】。

---

## 🌐 兼容性说明 (Compatibility Matrix)

本 Mod 采用**纯追加（Pure Additive）架构**：
- 未修改、覆写任何原版游戏文件；
- 通过 gui/scripted_widgets/ 挂载独立 GUI，绝不与任何界面 Mod 冲突；
- 完美适配《东亚与帝国》（TGP）、天朝官僚制、天朝霸权体系；
- 兼容游戏后续任意补丁版本（1.18.* / 1.19.* 及更高版本）；
- 支持中途加入存档，亦可随时安全卸载。

---

## 📦 安装与启用指引 (Installation Guide)

### 方式一：Steam 创意工坊（推荐）
在 Steam 创意工坊订阅本 Mod，并在 Paradox 官方启动器中将其加入 Playset 并启用。

### 方式二：本地手动安装
1. 下载仓库最新 Release 源码压缩包并解压；
2. 将 Mod 文件夹置入游戏的本地 Mod 目录：
   - **Windows**: Documents/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
   - **Linux**: ~/.local/share/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
   - **macOS**: ~/Documents/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
3. 将文件夹内的 ck3grandprotectorate.mod 放置于上一级 mod/ 目录下；
4. 打开游戏启动器并在播放集中启用。

---

## 🤝 参与贡献与反馈 (Contributing)

欢迎提交 Issue 与 Pull Request：
1. **提交 Issue**：反馈漏洞、建议或游戏截图；
2. **提交 Pull Request**：
   - 保持所有 .txt 和 .yml 文件采用 **UTF-8 with BOM** 编码；
   - 在 [CHANGELOG.md](CHANGELOG.md) 中登记变动。

---

## 📜 开源许可协议 (License)

本项目采用 [MIT License](LICENSE) 开源授权。  
Copyright (c) 2026 ShunbaoLi.