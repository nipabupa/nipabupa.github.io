---
title: 收藏
menu_id: explore
---

{% quot 资源网站 el:h2 %}

{% grid %} 
<!-- cell -->
{% link https://www.iconfont.cn/ IconFont：阿里巴巴矢量图库 icon:https://img.alicdn.com/imgextra/i2/O1CN01FF1t1g1Q3PDWpSm4b_!!6000000001920-55-tps-508-135.svg desc:false %}
<!-- cell -->
{% link https://godotengine.org/ GotDot：2D&3D游戏开发引擎 icon:https://godotengine.org/assets/favicon.svg desc:false %}
{% endgrid %}


{% quot 开源镜像 el:h2 %}

### PyPi与Pip源

{% link https://pypi.org/ PyPi：Python第三方软件包管理 icon:https://pypi.org/static/images/logo-small.8998e9d1.svg desc:false %}

1. 首先创建配置文件
- 对于Linux或macOS，配置文件通常在 {% kbd ~/.pip/pip.conf %}
- 对于Windows，配置文件通常在 {% kbd %HOME%\pip\pip.ini %}

2. 配置文件添加以下内容
```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

3. 可选的Pip源
- 清华大学：`https://pypi.tuna.tsinghua.edu.cn/simple`
- 阿里云：`https://mirrors.aliyun.com/pypi/simple/`
- 中国科技大学：`https://pypi.mirrors.ustc.edu.cn/simple/`
- 豆瓣：`http://pypi.douban.com/simple/`

{% quot GitHub仓库 el:h2 %}