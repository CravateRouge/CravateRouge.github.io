---
title: '钓鱼模拟'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: 钓鱼模拟
      text: 您知道91%的成功数据泄露始于鱼叉式网络钓鱼攻击吗？
      secondary_action:
        text: 什么是钓鱼攻击？
        url: "#definition"
      announcement:
        text: "了解我们如何"
        link:
          text: "钓鱼"
          url: "#process"
    design:
      css_class: "dark"
      background:
        color: "navy"
        image:
          # 将您的背景图片添加到 `assets/media/`。
          filename: constellation.svg
          size: "auto;background-repeat:repeat"
          filters:
            brightness: 0.5

  - block: cta-image-paragraph-custom
    id: definition
    content:
      items:
        - title: 钓鱼……那是什么？一种新的捕鱼技巧？
          image: phish.svg
          text: |
            钓鱼是试图通过伪装成可信实体来获取敏感信息（如用户名、密码和信用卡详细信息）的过程，通常通过批量电子邮件进行，试图绕过垃圾邮件过滤器。

            声称来自流行社交网站、银行、拍卖网站或IT管理员的电子邮件通常被用来诱骗毫无戒心的公众。这是一种犯罪性欺诈的社会工程形式。

  - block: features
    id: types
    content:
      title: 最常见的钓鱼类型
      text: |
        在已知的数百种钓鱼骗局中\
        以下是四种最常见的类型
      items:
        - name: 电子邮件钓鱼
          icon: envelope
          description: |
            在电子邮件钓鱼攻击中，紧迫感是主要特征。诈骗者向多个收件人发送看似合法的电子邮件，鼓励他们修改密码或更新个人信息和账户详细信息。
        - name: 短信钓鱼
          icon: chat-bubble-bottom-center-text
          description: |
            这种钓鱼策略与钓鱼电子邮件非常相似。黑客通过发送短信试图从个人那里窃取机密信息，要求他们回复或采取进一步行动。如果个人拒绝这样做，犯罪分子有时甚至会威胁他们。
        - name: 鱼叉式钓鱼
          icon: finger-print
          description: |
            这种策略需要使用电子邮件对特定个人或企业进行攻击。犯罪分子获取目标的个人信息，并利用这些信息向他们发送个性化且可信的电子邮件。
        - name: CEO 欺诈
          icon: identification
          description: |
            网络犯罪分子发送电子邮件，假装是C级高管或同事，通常要求进行资金转账或提供税务信息。
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
  
  - block: cta-image-paragraph-custom
    parity: "odd"
    content:
      items:
        - title: 什么是钓鱼模拟及其重要性？
          image: laptop-phish.png
          text: |
            钓鱼模拟是模仿现实世界钓鱼电子邮件的行为，组织可以向员工发送这些邮件以测试其在线行为并评估其对钓鱼攻击的知识水平。这些电子邮件反映了专业人士在日常活动中可能遇到的网络威胁，无论是在工作时间还是非工作时间。

            最新统计数据显示，钓鱼威胁持续上升。自2019年以来，钓鱼攻击的数量每年增长150%，反钓鱼工作组（APWG）报告称2022年钓鱼活动达到历史新高，记录了超过470万个钓鱼网站。根据Proofpoint的数据，2022年84%的组织经历了至少一次成功的钓鱼攻击。

            因为即使是最好的电子邮件网关和安全工具也无法保护组织免受所有钓鱼活动的侵害，组织越来越多地转向钓鱼模拟。精心设计的钓鱼模拟通过两种重要方式帮助减轻钓鱼攻击的影响。模拟为信息安全团队提供了教育员工更好地识别和避免现实生活中钓鱼攻击所需的信息。它们还帮助安全团队发现漏洞，改进整体事件响应，并减少成功钓鱼尝试导致的数据泄露和财务损失的风险。

  - block: defile
    id: process
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
    content:
      title: 我们的钓鱼模拟流程
      text: "该流程通常包括五个步骤："
      items:
        - name: 规划
          icon: calendar-days
          description: |
            我们首先定义目标并设定范围，决定使用哪种类型的钓鱼电子邮件以及模拟的频率。我们还将确定目标受众，包括细分特定群体或部门。
        - name: 起草
          icon: document-text
          description: |
            制定计划后，我们将创建逼真的模拟钓鱼电子邮件，这些邮件与真实的钓鱼威胁非常相似，基于暗网上的钓鱼模板和钓鱼工具包进行建模。我们会特别注意主题行、发件人地址和内容等细节，以制作逼真的钓鱼模拟。我们还会包括社会工程策略——甚至冒充（或“伪造”）高管或同事作为发件人——以增加员工点击电子邮件的可能性。
        - name: 发送
          icon: paper-airplane
          description: |
            一旦我们确定了内容，我们将通过安全方式向目标受众发送模拟钓鱼电子邮件，同时注意隐私。
        - name: 监控
          icon: eye
          description: |
            发送模拟恶意电子邮件后，我们将密切跟踪并记录员工如何与模拟电子邮件交互，监控他们是否点击链接、下载附件或提供敏感信息。
        - name: 分析
          icon: magnifying-glass
          description: |
            钓鱼测试后，我们将分析模拟中的数据以确定点击率和安全漏洞等趋势。随后，我们将立即向未通过模拟的员工提供反馈，解释他们如何正确识别钓鱼尝试以及如何避免未来的真实攻击。

  - block: cta-card
    content: 
      title: 教育是防止钓鱼攻击的最佳方式
      text: |
        实施钓鱼模拟培训是加强数据保护的理想方法。它将帮助您的员工保持警觉，了解他们可能遇到的所有与钓鱼相关的骗局。
      button:
        text: 订购您的模拟
        url: /zh/contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---