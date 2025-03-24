---
title: '家'
date: 2024-10-30
type: landing

design:
  # Default section spacing
  spacing: "1rem"

sections:
  - block: hero
    content:
      title: |
        严格守护\
        你的网络安全
      text: 🌎 全球范围内可提供解决方案 🌎
      primary_action:
        text: 联络我们
        url: "contact/"
        icon: rocket-launch
      secondary_action:
        text: 了解我们的服务
        url: /#services
      announcement:
        text: "查看我们的最新"
        link:
          text: "文章"
          url: "../articles/"
    design:
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      # For full-screen, add `min-h-screen` below
      css_class: "dark"
      background:
        color: "navy"
        image:
          # Add your image background to `assets/media/`.
          filename: constellation.svg
          size: "auto;background-repeat:repeat"
          filters:
            brightness: 0.5
  - block: resume-biography-3
    id: about
    content:
      username: admin

  - block: stats
    content:
      items:
        - statistic: "100+"
          description: |
            任务安全完成
        - statistic: "6年+"
          description: |
            实地经验
        - statistic: "1500+"
          description: |
            安全专家认可<a class="underline hover:no-underline" href="https://github.com/CravateRouge/bloodyAD" target="_blank" rel="noopener noreferrer">我们的工作</a>
    design:
      # Section background color (CSS class)
      css_class: "bg-gray-100 dark:bg-gray-900"
      # Reduce spacing
      spacing:
        padding: ["1rem", 0, "1rem", 0]
  
  - block: collection
    content:
      title: 他们谈论我们
      text: 看看行业对我们的评价
      filters:
        folders:
          - reference
    design:
      view: article-grid
      fill_image: false
      columns: 2

  - block: features
    id: services
    content:
      title: 我们的服务
      text: 🧱 构建你的安全成熟度 🧱
      items:
        - name: 审计
          icon: bolt
          description: 利用渗透测试评估您的安全体系验证其防范攻击者的能力
        - name: 安全的工作空间
          icon: cpu-chip
          description: 全新安全解决方案 守护您的数字工作空间
        - name: 保护
          icon: lock-closed
          description: 通过添加适应基础设施的措施 强化您的系统
        - name: 培养
          icon: academic-cap
          description: 网络安全对你来说将不再有任何秘密
        - name: 调查
          icon: magnifying-glass
          description: 进行袭击后的调查 了解发生了什么
        - name: 软件
          icon: beaker
          description: 获取根据您的需求量身定制的保护、检测和审核工作
      design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph
    id: services-details
    content:
      items:
        - title: 安全的工作空间
          text: 通过几个步骤保护您的基础架构
          feature_icon: cpu-chip
          features:
            - "统一并加强访问权限"
            - "集中管理您的IT资产"
            - "重新掌控您的数据 (遵守GDPR)"
            - "基于微软™技术(不需要第三方)"
          image: secureWorkspace.png
          button:
            text: "更多信息"
            url: "../articles/secure-workspace/"

        - title: 保护您的基础设施
          text: 通过添加适应基础设施的措施 强化您的系统
          feature_icon: lock-closed
          features:
            - "保证您的数据安全"
            - "为您的业务量身定制优化的解决方案"
            - "实施安全标准"
          # Upload image to `assets/media/` and reference the filename here
          image: protect.png
          button:
            text: "询问报价"
            url: "contact/"
        - title: 审核您的资产
          text: 利用渗透测试评估您的安全体系验证其防范攻击者的能力
          feature_icon: bolt
          features:
            - "根据您的需求 使用自动化工具和高级攻击 通过测试来识别应用程序中的弱点"
            - "利用攻击模拟测试 检验您对威胁源的抵御韧性"
          # Upload image to `assets/media/` and reference the filename here
          image: audit.png
          button:
            text: "立即订购"
            url: "contact/"

        - title: 训练你的团队
          text: 网络安全对你来说将不再有任何秘密
          feature_icon: academic-cap
          features:
            - "通过邮件进行网络钓鱼活动提高团队的安全意识"
            - "学习开发安全软件"
            - "针对您需求的特定安全主题的培训"
          # Upload image to `assets/media/` and reference the filename here
          image: training.png
          button:
            text: "立即预订"
            url: "contact/"

        - title: 进行袭击后调查
          text: 进行袭击后的调查,了解发生了什么?
          feature_icon: magnifying-glass
          features:
            - "确定威胁行为者所走的路径并加以保护"
            - "评估损失"
            - "建立攻击者档案"
          # Upload image to `assets/media/` and reference the filename here
          image: investigate.png
          button:
            text: "立即请求"
            url: "contact/"
        
        - title: 自定义软件
          text: 获取根据您的需求量身定制的保护、检测和审核工具
          feature_icon: beaker
          features:
            - "使用自定义补丁保护您的软件免受漏洞"
            - "检测有针对性的威胁"
            - "PoC漏洞和恶意软件来模拟威胁"
          # Upload image to `assets/media/` and reference the filename here
          image: craft.png
          button:
            text: "询问报价"
            url: "contact/"
    design:
      # Section background color (CSS class)
      css_class: "bg-gray-100 dark:bg-gray-900"

  # - block: testimonials
  #   content:
  #     title: ""
  #     text: ""
  #     items:
  #       - name: "Hugo Smith"
  #         role: "Marketing Executive at X"
  #         # Upload image to `assets/media/` and reference the filename here
  #         image: "testimonial-1.jpg"
  #         text: "Awesome, so easy to use and saved me so much work with the swappable pre-designed sections!"
  #   design:
  #     spacing:
  #       # Reduce bottom spacing so the testimonial appears vertically centered between sections
  #       padding: ["6rem", 0, 0, 0]

  - block: cta-card
    content:
      title: "加强您的基础设施"
      text: "我们的团队已经着手处理此事!"
      button:
        text: "联络我们"
        url: "contact/"
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
