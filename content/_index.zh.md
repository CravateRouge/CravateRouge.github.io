---
title: '家'
type: landing

design:
  # Default section spacing
  spacing: "0rem"

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
      css_class: "dark"
      background:
        color: "navy"
        image:
          # Add your image background to `assets/media/`.
          filename: constellation.svg
          position: repeat
          size: auto
          filters:
            brightness: 0.5
      spacing: 
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
  - block: resume-biography-3
    id: about
    design:
      spacing: 
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      name:
        size: sm
    content:
      username: admin
      headings:
        about: 我们是谁？
      text: |
        我是李白兔，卡瓦特网络安全公司的首席执行官和安全专家。

        我在[巴黎-萨克雷大学](https://www.shanghairanking.com/institution/paris-saclay-university)获得硕士学位后，开始了我的网络安全之旅。随后，我在[工银安盛人寿](https://www.icbc-axa.com/)担任红队操作员，专注于微软环境。这一角色让我有机会模拟真实世界的攻击，以识别和减轻漏洞。

        此外，我分享我的研究并开发先进的工具，帮助安全专家为您提供最前沿的保护。特别是，我有幸在世界上最大的计算机安全事件之一[拉斯维加斯黑帽大会](https://www.blackhat.com/us-22/arsenal/schedule/#bloodyad-26883)上展示了我的一款工具。

        这就是为什么我决定创办卡瓦特网络安全公司，将我的专业技术直接带给您。通过个性化、全面的网络安全服务，专注于微软产品，我们确保您的数字资产由业内最优秀的人才保护。相信我们的专业技术，保障您的未来无后顾之忧！
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
            安全专家认可<a href='https://github.com/CravateRouge/bloodyAD'>我们的工作</a>
    spacing:
      padding: ["1rem", 0, "1rem", 0]
    design:
      # Section background color (CSS class)
      css_class: "bg-gray-100 dark:bg-gray-900"
  
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
      spacing:
        padding: ["1rem", 0, 0, 0]

  - block: features
    id: services
    content:
      title: 我们的服务
      text: 🧱 构建你的安全成熟度 🧱
      items:
        - name: 审计
          icon: bolt
          description: |
            <a href='#auditsum'>利用渗透测试评估您的安全体系验证其防范攻击者的能力</a>
        - name: 保护
          icon: lock-closed
          description: |
            <a href='#protectsum'>通过添加适应基础设施的措施 强化您的系统</a>
        - name: 培养
          icon: academic-cap
          description: |
            <a href='#trainingsum'>网络安全对你来说将不再有任何秘密</a>
        - name: 网络钓鱼模拟
          icon: envelope
          description: |
            <a href='#phishingsum'>通过实施强大的网络钓鱼模拟训练您的员工识别网络威胁</a>
        - name: Microsoft 365 & Azure 集成
          icon: cpu-chip
          description: |
            <a href='#m365sum'>将 M365 和 Azure 云集成到您的业务中 以提高生产力和安全性</a>
        - name: 定制服务
          icon: beaker
          description: |
            <a href='#tailoredsum'>如果您有特定需求 请告知我们</a>
      design:
        css_class: "bg-gray-100 dark:bg-gray-900"
        spacing:
        padding: [0, 0, 0, 0]

  - block: cta-image-paragraph
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      items:
        - title: 审核您的资产
          text: 利用渗透测试评估您的安全体系验证其防范攻击者的能力
          feature_icon: bolt
          features:
            - "根据您的需求 使用自动化工具和高级攻击 通过测试来识别应用程序中的弱点"
            - "利用攻击模拟测试 检验您对威胁源的抵御韧性"
          image: audit.png
          button:
            text: "了解更多"
            url: "services/audit/"
          id: auditsum
        - title: 保护您的基础设施
          text: 通过添加适应基础设施的措施 强化您的系统
          feature_icon: lock-closed
          features:
            - "保证您的数据安全"
            - "为您的业务量身定制优化的解决方案"
            - "实施安全标准"
          image: protect.png
          button:
            text: "询问报价"
            url: "contact/"
          id: protectsum
        - title: 训练你的团队
          text: 网络安全对你来说将不再有任何秘密
          feature_icon: academic-cap
          features:
            - "学习开发安全软件"
            - "针对您需求的特定安全主题的培训"
          image: training.png
          button:
            text: "立即预订"
            url: "contact/"
          id: trainingsum
        - title: 模拟网络钓鱼攻击
          text: 提高您团队的安全意识
          feature_icon: envelope
          features:
            - "反映现实世界的网络威胁"
            - "教育终端用户常见的网络钓鱼策略"
            - "可定制的网络钓鱼场景"
          image: phishing.png
          button:
            text: "了解更多"
            url: services/phishing
          id: phishingsum
        - title: Microsoft 365 集成
          text: 一体化解决方案 提高您的生产力和安全性
          feature_icon: cpu-chip
          features:
            - "统一并加强访问权限"
            - "集中管理您的IT资产"
            - "重新掌控您的数据 (遵守GDPR)"
            - "基于微软™技术(不需要第三方)"
          image: secureWorkspace.png
          button:
            text: "更多信息"
            url: services/integration/
          id: m365sum
        - title: 定制服务
          text: 有特定的网络安全需求？
          feature_icon: beaker
          image: craft.png
          button:
            text: 告诉我们您的项目
            url: "contact/"
          id: tailoredsum

  - block: cta-card
    content:
      title: "加强您的基础设施"
      text: "我们的团队已经着手处理此事!"
      button:
        text: "联络我们"
        url: "contact/"
    design:
      spacing:
        padding: ["1rem", 0, 0, 0]
---
