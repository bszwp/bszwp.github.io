---
layout: post
title: "Sublime Text自定义配色方案"
date: 2016-12-02
tags: 工具
---



先推荐介绍几个自定义主题: 

[https://github.com/aziz/tmTheme-Editor](https://github.com/aziz/tmTheme-Editor)
[http://tmtheme-editor.herokuapp.com/](http://tmtheme-editor.herokuapp.com/)
这个可以在线修改配色方案,也可以上传本地的方案修改.

------

[https://github.com/LeonardoGentile/ColorSchemeEditorLive](https://github.com/LeonardoGentile/ColorSchemeEditorLive)
随意打开一个文档然后shift+f12(打开当前配色方案)
然后把布局换成两列(左侧你的文档,右侧配色方案)
这时当你点击左侧文档中的代码,右侧会跳到当前代码配色范围中

------

[https://github.com/MattDMo/Neon-color-scheme](https://github.com/MattDMo/Neon-color-scheme)
这个配色方案支持很多很多很多很多.....语言的语法
这样就不需要自己写范围了

把Neon color scheme里面不需要的范围删掉,然后再用tmThemeEditor在线编辑,最后用Color Scheme Editor做小修改.

------

### 使用Seti_UI自定义主题

#### 主题安装

使用package control搜索Seti_UI进行安装，默认会将SetiUI-Icons装上

#### 文件图标

https://github.com/mrmartineau/SetiUI-Icons-Sublime

如果不想使用SetiUI而喜欢其图标，可以单独下载图标进行安装

#### 修改主题

先看一下动大刑之后的sublime

![](http://ondh71tpt.bkt.clouddn.com/img/posts/sublime/01.png)

开始修改

在"首选项-设置"中打开设置文件，在用户文件中进行主题配置，seti有很多选项可以选择。

具体参数参见:  https://github.com/jesseweed/seti-ui

```json
{
	"Seti_ClosedFolder_anim": true,
	"Seti_SB_big": true,
	"Seti_SB_med": true,
	"Seti_accent_lime": true,
	"Seti_alt_tree_row": true,
	"Seti_bold_heading": true,
	"Seti_bold_slctdfile_labels": true,
	"Seti_bold_slctdtab_labels": true,
	"Seti_lime_label": true,
	"Seti_lime_map": true,
	"Seti_lime_scrollbar": true,
	"Seti_lime_statusbar": true,
	"Seti_lime_tabclose": true,
	"Seti_no_blue_bar": true,
	"Seti_red_tab": true,
	"Seti_sb_small_padding": true,
	"Seti_sb_tree_miny": true,
	"Seti_show_group_arrows": true,
	"Seti_sidebar_font_size_12": true,
	"Seti_tab_font_12": true,
	"Seti_tabs_big": true,
	"Seti_tabs_med": true,
	"Seti_tabs_small": true,
	"Seti_top_heading_anim": true,
	"color_scheme": "Packages/my Color Scheme/Lyte-Solarized-Light (SL).tmTheme",
	"font_size": 16,
	"highlight_line": true,
	"ignored_packages":
	[
		// 忽略的包
	],
	"mouse_wheel_tabswitch": true,
	"tab_size": 4,
	"theme": "Seti.sublime-theme",
	"translate_tabs_to_spaces": true
}

```



在"首选项>浏览插件目录"进入sublime的插件目录，进入Seti_UI目录。

目录结构如下

```tree
Seti_UI                        
├─icons                    
│  ├─Langs                 
│  └─Prefs                 
├─Main                     
│  ├─accent                
│  │  ├─indigo             
│  │  │  └─orig            
│  │  ├─lime               
│  │  │  └─orig            
│  │  ├─purple             
│  │  │  └─orig            
│  │  ├─red                
│  │  │  └─orig            
│  │  ├─seablue            
│  │  │  └─orig            
│  │  ├─teal               
│  │  │  └─orig            
│  │  └─yellow             
│  │      └─orig           
│  ├─menu                  
│  └─sidebar               
├─messages                 
├─Resource                 
├─Widgets          
└─Seti.sublime-theme
```



使用sublime修改Seti.sublime-theme文件，可以看到实时的界面更改

大概分为以下模块:

```json
TABS (REGULAR)
TAB BUTTONS
TAB LABELS tab
TAB SCROLLING
FOLD BUTTONS
STANDARD SCROLLBARS
OVERLAY SCROLLBARS
EMPTY WINDOW BACKGROUND
GRID LAYOUT
MINI MAP
DIALOG
PROGRESS BAR
LABELS
TOOLTIP
STATUS BAR
SIDEBAR
SIDEBAR - OPEN FILE ICONS
SIDEBAR - GENERAL FILE ICONS
STANDARD TEXT BUTTONS
TEXT INPUT FIELD
PANEL BACKGROUNDS
MINI QUICK PANEL
QUICK PANEL
CODE COMPLETION DROPDOWN
BOTTOM PANEL BUTTONS
BOTTOM PANEL ICONS - GROUP 1
BOTTOM PANEL ICONS - GROUP 2
BOTTOM PANEL ICONS - GROUP 3
BOTTOM PANEL ICONS - GROUP 4
Accents
```

里面的设置基本上可以见名知意，很容易修改。比如其中一段修改侧边栏样式的代码 : 

```json
    {
        "class": "sidebar_container",
        "settings": ["Seti_top_heading_anim"],
        "layer0.tint": [255,255,204], //侧边栏背景颜色
        "layer0.inner_margin": [0, 54, 0, 0],
        "layer1.texture": {
            "keyframes": [
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h8.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h7.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h6.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h5.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h4.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h3.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h2.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h1.png",
                "Seti_UI/Main/sidebar/sidebar_heading_alt-h0.png"
            ],
            "loop": false,
            "frame_time": 0.030
        },
        "layer1.opacity": { "target": 1.0, "speed": 10.0, "interpolation": "smoothstep" },
        "layer1.inner_margin": [120, 54, -120, 0],
        "content_margin": [0, 54, 0, 0]
    }
```

好了，可以动大刑改变自己的sublime了！









