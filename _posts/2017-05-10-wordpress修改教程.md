---
layout: post
title: "wordpress修改教程"
date: 2017-05-10
tags: wordpress
---

WordPress后台很多模块有时并不需要，使用下面的代码可以将它们屏蔽掉。

根据需要，将下面代码添加到当前主题functions.php模板文件中：

### 屏蔽左侧菜单

```php
function remove_menus() {
    global $menu;
    $restricted = array(
        __('Dashboard'),
        __('Posts'),
        __('Media'),
        __('Links'),
        __('Pages'),
        __('Appearance'),
        __('Tools'),
        __('Users'),
        __('Settings'),
        __('Comments'),
        __('Plugins')
    );
    end ($menu);
    while (prev($menu)){
        $value = explode(' ',$menu[key($menu)][0]);
        if(strpos($value[0], '<') === FALSE) {
            if(in_array($value[0] != NULL ? $value[0]:"" , $restricted)){
                unset($menu[key($menu)]);
            }
        }else {
        $value2 = explode('<', $value[0]);
            if(in_array($value2[0] != NULL ? $value2[0]:"" , $restricted)){
                unset($menu[key($menu)]);
            }
        }
    }
}
if (is_admin()){
    // 屏蔽左侧菜单
    add_action('admin_menu', 'remove_menus');
}
```



### 屏蔽后台更新模块

```php
function wp_hide_nag() {
    remove_action( 'admin_notices', 'update_nag', 3 );
}
add_action('admin_menu','wp_hide_nag');
```



### 屏蔽 WP 后台“显示选项”和“帮助”选项卡

```php
function remove_screen_options(){ return false;}
    add_filter('screen_options_show_screen', 'remove_screen_options');
    add_filter( 'contextual_help', 'wpse50723_remove_help', 999, 3 );
    function wpse50723_remove_help($old_help, $screen_id, $screen){
    $screen->remove_help_tabs();
    return $old_help;
}
```



### 屏蔽后台仪表盘无用模块

```php
function example_remove_dashboard_widgets() {
    // Globalize the metaboxes array, this holds all the widgets for wp-admin
    global $wp_meta_boxes;
    // 以下这一行代码将删除 "快速发布" 模块
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press']);
    // 以下这一行代码将删除 "引入链接" 模块
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_incoming_links']);
    // 以下这一行代码将删除 "插件" 模块
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_plugins']);
    // 以下这一行代码将删除 "近期评论" 模块
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_comments']);
    // 以下这一行代码将删除 "近期草稿" 模块
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_recent_drafts']);
    // 以下这一行代码将删除 "WordPress 开发日志" 模块
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_primary']);
    // 以下这一行代码将删除 "其它 WordPress 新闻" 模块
    unset($wp_meta_boxes['dashboard']['side']['core']['dashboard_secondary']);
    // 以下这一行代码将删除 "概况" 模块
    unset($wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now']);
}
add_action('wp_dashboard_setup', 'example_remove_dashboard_widgets' );
```



### 屏蔽后台页脚版本信息

```php
function change_footer_admin () {return '';}
add_filter('admin_footer_text', 'change_footer_admin', 9999);
function change_footer_version() {return '';}
add_filter( 'update_footer', 'change_footer_version', 9999);
```



### 屏蔽后台左上LOGO

```php
function annointed_admin_bar_remove() {
        global $wp_admin_bar;
        /* Remove their stuff */
        $wp_admin_bar->remove_menu('wp-logo');
}
add_action('wp_before_admin_bar_render', 'annointed_admin_bar_remove', 0);
```



### 去除后台标题中的 “—— WordPress”

```php
add_filter('admin_title', 'wpdx_custom_admin_title', 10, 2);
function wpdx_custom_admin_title($admin_title, $title){
    return $title.' &lsaquo; '.get_bloginfo('name');
}
```



### 修改汉化文件

用Poedit打开 wp-content\languages\ 目录下的zh_CN.po或.po文件来修改.主题目录下还有一个languages目录，这里也有一些.po文件，根据需要进行修改。

Poedit下载地址:   https://poedit.net/



### 去除谷歌字体

自从谷歌被墙了，就连谷歌字体也链接不上了，但是WordPress后台和一些外国主题都使用了谷歌字体，谷歌字体链接不上自然就导致了WordPress奇慢无比，所以在国内使用WordPress做的第一件事就是去掉谷歌字体。

方法一:

将以下代码添加到当前使用的主题的functions.php文件中即可。

```php
if (!function_exists('remove_wp_open_sans')) :
  function remove_wp_open_sans() {
    wp_deregister_style( 'open-sans' );
    wp_register_style( 'open-sans', false );
  }
  // 前台删除Google字体CSS
  add_action('wp_enqueue_scripts', 'remove_wp_open_sans');
  // 后台删除Google字体CSS
  add_action('admin_enqueue_scripts', 'remove_wp_open_sans');
endif;

```

方法二: 

使用Disable Google Fonts插件

方法三:

删除代码：很多人不喜欢用插件那么这个可以修改代码打开/wp-includes/script-loader.php搜索fonts.googleapis.com找到代码位置，直接把//fonts.googleapis.com/…这个链接整个删掉即可。（修改前请备份好数据，用//注释掉也是可以的哦）

方法四:

依次打开 /wp-includes/script-loader.php，约581行的位置，替换谷歌的字体库为360网站卫士的国内CDN节点（推荐）将上图截图中箭头指示的fonts.googleapis.com换成fonts.useso.com即可。



### 去掉插件造成的新增菜单

比如网易云音乐，找到add_menu_page字段进行修改

打开wp-content/plugins/netease-music/function/core.php进行修改

```php
function nm_menu() {
    // add_menu_page( 'neteasemusic', '网易云音乐', 'manage_options', 'neteasemusic', 'nm_setting_page' );
    // add_submenu_page( 'neteasemusic', '设置', '设置', 'manage_options', 'neteasemusic', 'nm_setting_page' );
    // add_submenu_page( 'neteasemusic', '自定义歌单', '自定义歌单', 'manage_options', 'neteasemusic-list', 'nm_list_setting' );
    // add_submenu_page( 'neteasemusic', '帮助', '帮助', 'manage_options', 'neteasemusic-help', 'nm_setting_page' );
    // add_action( 'admin_init', 'nm_setting_group');
}
```



### 修改头像

找到wp-admin文件夹下的user-edit.php，注释或删除以下语句

```php
<?php echo get_avatar( $user_id ); ?>
<p class="description"><?php
  if ( IS_PROFILE_PAGE ) {
    /* translators: %s: Gravatar URL */
    $description = sprintf( __( 'You can change your profile picture on <a href="%s">Gravatar</a>.' ),
                           __( 'https://en.gravatar.com/' )
                          );
  } else {
    $description = '';
  }

/**
			 * Filters the user profile picture description displayed under the Gravatar.
			 *
			 * @since 4.4.0
			 * @since 4.7.0 Added the `$profileuser` parameter.
			 *
			 * @param string  $description The description that will be printed.
			 * @param WP_User $profileuser The current WP_User object.
			 */
echo apply_filters( 'user_profile_picture_description', $description, $profileuser );
?></p>
```

可以修改为

```php
<img alt="" src="<?php bloginfo('template_directory'); ?>/images/avatar.png" class="avatar avatar-96 photo" height="96" width="96">
```



### 让WordPress支持注册用户上传自定义头像功能

使用插件Simple Local Avatars和 WP User Avatar







参考资料:

[屏蔽WordPress后台无用项](http://zmingcx.com/shielding-wordpress-useless-items.html)

[WordPress修改仪表盘名称及汉化教程](http://www.myzhenai.com.cn/post/851.html)

[WordPress优化加速:去掉谷歌字体](http://www.ymjihe.com/1694.html)

[为wordpress添加本地头像功能代替Gravatar](http://www.v7v3.com/wpjiaocheng/2014051161.html)







