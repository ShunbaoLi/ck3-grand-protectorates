# CK3 Grand Protectorates (天子敕建大都护府)

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-ck3--grand--protectorates-181717.svg?logo=github)](https://github.com/ShunbaoLi/ck3-grand-protectorates)
[![Changelog](https://img.shields.io/badge/Changelog-Keep_a_Changelog-blueviolet.svg)](CHANGELOG.md)
[![CK3 Version](https://img.shields.io/badge/CK3_Version-1.18%20%7C%201.19+-orange.svg)](https://ck3.paradoxwikis.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[简体中文](README.md) | [English](README.en.md)**

</div>

---

## 📌 Introduction

**CK3 Grand Protectorates** is a premium, modular, and decoupled administrative gameplay mod designed specifically for Crusader Kings III and the Celestial Hegemony (Tier 6) mechanics.

In vanilla CK3 and official DLC expansions such as *Tours & Tournaments* and *The Great Project (East Asia & Empires)*, managing vast frontier domains often lacks the historical grandeur of the Tang Dynasty's Grand Protectorate system. Relying on vanilla century-long de jure drift is far too slow and inflexible for rapidly expanding empires.

This mod empowers the Celestial Emperor (Son of Heaven) with absolute imperial authority to establish and realign frontier jurisdictions:
- **Four Independent Imperial-Tier (Tier 5) Grand Protectorates** (Anxi, Andong, Anbei, Annan);
- A dedicated **Multi-Select Management Panel** with native checkboxes to flexibly reassign kingdom de jure status;
- A complete **Ancestral Empire Memory & Safe Return Loop**, alongside a dynamic **Decommission Mechanism** when jurisdictions are cleared;
- Deep integration with Celestial bureaucracy: automatically binds the **Protectorate Subject Contract (celestial_province_protectorate)** and **Governor Succession Law (celestial_military_appointment_succession_law)**, granting appointed rulers the title of "Grand Protector" (大都护).

> 📜 **Release History**: See [CHANGELOG.md](CHANGELOG.md) for full details.

---

## 🏛️ The Four Grand Protectorates Matrix

To guarantee 100% zero conflict with vanilla files and historical bookmarks, this mod uses completely decoupled title IDs (e.g. e_andong_protectorate does not overwrite or touch vanilla e_andong):

| Grand Protectorate | Title ID | Historic Seat | Geographic Orientation | Government & Succession |
| :--- | :--- | :--- | :--- | :--- |
| **Anxi Grand Protectorate** | e_anxi_protectorate | Xizhou (c_xizhou) | Western Regions, Tarim Basin, Transoxiana, Persia, Tibet | Celestial Protectorate Contract · Governor Succession |
| **Andong Grand Protectorate** | e_andong_protectorate | Youzhou (c_youzhou) | Liaodong, Korean Peninsula, Japanese Archipelago | Celestial Protectorate Contract · Governor Succession |
| **Anbei Grand Protectorate** | e_anbei_protectorate | Wuyuan (c_wuyuan) | Mongolian Steppes, Lake Baikal, Siberian Tundra | Celestial Protectorate Contract · Governor Succession |
| **Annan Grand Protectorate** | e_annan_protectorate | Jiaozhou / Longbian (c_thang_long) | Lingnan Outer Frontiers, Indochina, Southeast Asia | Celestial Protectorate Contract · Governor Succession |

---

## ⚙️ Workflow & De Jure Data Loop

`mermaid
flowchart TD
    A[Celestial Emperor / Hegemon] -->|Take Decision| B(Establish Grand Protectorate)
    B --> C{Select Cardinal Direction}
    C -->|Anxi / Andong / Anbei / Annan| D[Open Dedicated Management GUI]
    
    D --> E[Check Candidate Kingdoms]
    E -->|Record original_de_jure_empire| F[Shift De Jure via set_de_jure_liege_title]
    
    D --> G[Uncheck Existing Kingdoms]
    G -->|Read original_de_jure_empire| H[Safely Revert to Ancestral Mother Empire]
    
    D --> I[Uncheck All Kingdoms Count=0]
    I -->|Trigger Warning| J[Decommission Title via destroy_title & Refund Prestige]
`

### 1. Decision: [Imperial Edict: Establish Grand Protectorate]
- **Requirements**: Player character (is_ai = no), holds Celestial Hegemony (	ier_hegemony), and controls at least one eligible non-Zhongyuan kingdom in the frontier direction.
- **Establishment**: Select a direction via event, open the management window, select constituent kingdoms, and instantiate the imperial title with Governor Succession.

### 2. Decision: [Realign Grand Protectorate Jurisdiction]
- Once established, cardinal realignment decisions open at will.
- **Pre-Selected Kingdoms**: Currently attached kingdoms appear checked [√].
- **Candidate Kingdoms**: Eligible kingdoms in your realm appear unchecked [ ].
- **Safe Reversion**: Kingdoms removed from the list safely return to their recorded ancestral mother empire (even if that empire is uncreated or dormant).
- **Decommissioning**: Unchecking all kingdoms and confirming safely destroys the protectorate and refunds 300 prestige.
- **Personnel Decoupling**: De jure jurisdiction is decoupled from vassal transfer, allowing natural synergy with vanilla vassal transfer mechanics.

---

## 🖥️ UI & Component Standards

The dedicated interface (gui/window_grand_protectorate.gui) adheres strictly to vanilla design standards:
1. **Checkboxes**: Direct reuse of standard utton_checkbox from Great Projects;
2. **Title Cards**: Grant-title card styling featuring coa_title_tiny_crown, kingdom name, ancestral empire, and duchy count;
3. **Decommission Warning**: High-visibility banner when all items are unchecked;
4. **Primary Confirmation Button**: Prominent golden utton_primary_big ("Enact Imperial Edict: Demarcate Frontier").

---

## 🌐 Compatibility

- **Pure Additive Architecture**: No vanilla game files are overwritten.
- **Independent GUI Registration**: Custom HUD window registered via gui/scripted_widgets/ ensuring zero UI mod conflicts.
- **Expansion Compatible**: Fully compatible with *The Great Project*, *Roads to Power*, and upcoming Paradox patches (1.18.*, 1.19.*+).
- **Save Game Safe**: Can be added to existing saves safely.

---

## 📦 Installation

### Option 1: Steam Workshop (Recommended)
Subscribe to the mod on the Steam Workshop, then enable it in your Paradox Launcher Playset.

### Option 2: Manual Installation
1. Download and extract the latest repository release package.
2. Place the folder into your local mod folder:
   - **Windows**: Documents/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
   - **Linux**: ~/.local/share/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
   - **macOS**: ~/Documents/Paradox Interactive/Crusader Kings III/mod/ck3grandprotectorate
3. Ensure ck3grandprotectorate.mod resides in the parent mod/ directory.
4. Launch the game launcher and activate the mod in your playset.

---

## 📜 License

Distributed under the [MIT License](LICENSE).  
Copyright (c) 2026 ShunbaoLi.