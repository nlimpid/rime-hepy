昨晚因为突然想试试 @polyhedron 的中古汉语全拼于是终于上手尝试口碑极佳的鼠须管，一头雾水地折腾到凌晨五点多。最惊喜的是发现鼠须管自带 X-SAMPA 输入方案！tsaijɛ pupʰa ʂuɻu kuotɕijinpiɑu lə…
以下分享一些我目前使用的自定义配置。


## squirrel.custom.yaml

* 首先让鼠须管的部署状态等通知使用 OS X 自己的通知中心，不要用 Growl。
然后是鼠须管的默认样式实在不好看，众多自带方案也都很简陋（只有一个 Starcraft 方案还有点意思）。于是我写了个很干净的方案自用——呃，很「冷漠」的方案，这个样子：
![](http://pic3.zhimg.com/62dfe1ccfedcb84a06abc2ce39d14ed3_b.jpg)
* 然后是菜单栏图标也不好看，所以也自制了一个。虽说只是随手做的草稿，还没做什么像素对齐的优化，但感觉已经很不错了，效果如下（安装此图标的方法详见：[Issue 500](https://code.google.com/p/rimeime/issues/detail?id=500 "Issue 500")）：
![](http://pic1.zhimg.com/a5e070d3d1769200ea53926542b7f8ff_b.jpg)
* 哦对了我还把 Squirrel.app 的图标换成了系统默认的 bundle / kernel extension 图标……为了让部署状态通知好看一些……（这个图标可以在 /System/Library/CoreServices/CoreTypes.bundle/Contents/Resources/KEXT.icns 找到。）
![](http://pic1.zhimg.com/533f43a29b12f700a5fc99cb6c24df6e_b.jpg)
以下是自定义通知行为和候选条样式的代码：

# squirrel.custom.yaml

	patch:

  	show_notifications_via_notification_center: true

  	style:
    	color_scheme: apathy

  	preset_color_schemes:

	    apathy:
	
	      name: "冷漠 / Apathy"
	      author: "LIANG Hai <___@___.com>"
	
	      horizontal: true
	      inline_preedit: true
	        # 单行模式
	      candidate_format: "%c\u2005%@\u2005"
	        # 用 1/6 em 空格 U+2005 来控制编号 %c 和候选词 %@ 前后的空间。
	
	      corner_radius: 2
	      border_height: 5
	      border_width: 7
	      back_color: 0xFFFFFF
	
	      font_face: "Lucida Grande"
	        # 我的系统简中字体已设定为冬青黑体；
	        # 此处设定西文字体让它自动回退，以免候选词里的西文用中文字体显示。
	      font_point: 18
	      text_color: 0xAAAAAA
	
	      comment_text_color: 0xDDDDDD
	
	      label_font_face: "Lucida Grande"
	      label_font_point: 17
	      # label_color: 0xDDDDDD
	        # label_color 这个属性当前有 bug；
	        # 每当开启「方案选单」，
	        # 它就会被 text_color 和 back_color 的中和色替换掉。
	
	      hilited_candidate_text_color: 0xEE6E00
	      hilited_candidate_back_color: 0xFFFFFF
	        # 令高亮候选词的背景色与候选条背景色 back_color 一致
	      # hilited_candidate_label_color: 0xEE6E00
	        # 问题同 label_color

## default.custom.yaml

启用的输入法方案：小鹤双拼、中古汉语全拼、X-SAMPA 国际音标
（参考自官方指引：Customization Guide: 在方案選單中添加五筆、雙拼）
允许使用左右方括号翻页。
（参考自官方指引：Customization Guide: 以方括號鍵換頁）
# default.custom.yaml

	patch:

  	schema_list:
    	# - schema: luna_pinyin         # 朙月拼音
    	# - schema: luna_pinyin_simp    # 朙月拼音 简化字模式
    	# - schema: luna_pinyin_tw      # 朙月拼音 臺灣正體模式
	    # - schema: terra_pinyin        # 地球拼音 dì qiú pīn yīn
	    # - schema: bopomofo            # 注音
	    # - schema: jyutping            # 粵拼
	    # - schema: cangjie5            # 倉頡五代
	    # - schema: cangjie5_express    # 倉頡 快打模式
	    # - schema: quick5              # 速成
	    # - schema: wubi86              # 五笔86
	    # - schema: wubi_pinyin         # 五笔拼音混合輸入
	    # - schema: double_pinyin       # 自然碼雙拼
	    # - schema: double_pinyin_mspy  # 微軟雙拼
	    # - schema: double_pinyin_abc   # 智能ABC雙拼
	    - schema: double_pinyin_flypy   # 小鶴雙拼
	    # - schema: wugniu              # 吳語上海話（新派）
	    # - schema: wugniu_lopha        # 吳語上海話（老派）
	    # - schema: sampheng            # 中古漢語三拼
	    - schema: zyenpheng             # 中古漢語全拼
	    - schema: ipa_xsampa            # X-SAMPA 國際音標
	    # - schema: emoji               # Emoji

  	key_binder/bindings:
    	- { when: paging, accept: bracketleft, send: Page_Up }
    	- { when: has_menu, accept: bracketright, send: Page_Down }

## double_pinyin_flypy.custom.yaml

禁止将双拼的候选字符串（即「preedit」）显示为拼音（比如 ll hd 被显示为 liang hai）。这是为了避免在中文状态下临时输入西文时被自动转换的拼音干扰（比如 zhihu 会被显示为 zang chang sh）。
# double_pinyin_flypy.custom.yaml

patch:

  translator/preedit_format: {}
    # 实际上是清空了原有的 preedit_format 定义