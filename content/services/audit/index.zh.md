---
title: '审计'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: 渗透测试服务
      text: 通过值得信赖的网络安全专家提供的基础设施保护服务，了解您的安全控制的强度
      secondary_action:
        text: 了解方法论
        url: "#methodology"
      announcement:
        text: "了解我们的渗透测试"
        link:
          text: "服务"
          url: "#services"
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

  - block: features
    id: services
    content:
      title: 您的渗透测试范围是什么？
      text: 测试您的组织的全部或部分
      items:
        - name: 外部
          icon: share
          description: |
            测试您的暴露信息系统的全部或部分的安全强度，这些系统可能会被黑客或恶意软件利用。
        - name: 内部
          icon: building-office-2
          description: |
            测试您的公司对具有某些访问权限的内部攻击者的安全强度（VPN访问、钓鱼攻击妥协、Wi-Fi访问等）。非常适合测试您的Active Directory、网络和Wi-Fi的安全性。
        - name: Web/应用程序
          icon: globe-alt
          description: |
            通过检测OWASP定义的漏洞来评估您的应用程序和网站的安全性。
        - name: 云
          icon: cloud
          description: |
            评估您的私有云或公共云提供商（Azure、O365、AWS、GCP）的安全性。
        - name: 红队
          icon: user
          description: |
            模拟现实攻击以测试您的组织对高级入侵场景的抵抗力。
        - name: 紫队
          icon: users
          description: |
            通过与SOC（安全运营中心）合作，结合攻击和防御的力量，提高您的威胁检测和响应能力。
        - name: 移动
          icon: device-phone-mobile
          description: |
            通过识别漏洞并保护敏感数据，评估您的Android和iOS应用程序的安全性。
        - name: AV/EDR
          icon: eye
          description: |
            分析部署在您的网络上的EDR/防病毒软件的覆盖范围和配置，以提高其在检测攻击方面的有效性。
        - name: 物理入侵
          icon: wrench-screwdriver
          description: |
            发现您公司场所物理安全中的弱点，例如未锁定的门、不足的监控系统或不适当的访问程序。
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph-custom
    id: methodology
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      title: 渗透测试的方法论
      text: 选择适合您需求和限制的方法。
      items:
        - title: 黑盒
          image: black-box.svg
          text: |
            黑盒测试是在对目标环境没有任何先验知识的情况下进行渗透测试。这种方法复制了一个现实的外部攻击，主要测试暴露给公众的攻击面，例如Web应用程序或开放系统。这种方法衡量公司在没有初始权限的情况下抵御外部攻击者的真实能力。然而，它可能在识别复杂的内部漏洞方面缺乏深度，这可以通过白盒或灰盒方法来弥补，这些方法更精确地探索内部漏洞。
        - title: 白盒
          image: white-box.svg
          text: |
            白盒审计是在拥有目标基础设施的所有必要信息的情况下进行渗透测试：源代码、网络架构、系统配置等。这种方法可以快速识别难以通过其他方式检测到的深层次和复杂漏洞。它提供了全面的测试覆盖，优化了资源和时间。然而，这种方法在外部攻击者无法获得此类信息的情况下可能缺乏现实性，这时灰盒或黑盒测试可以通过模拟更现实的外部攻击来补充。
        - title: 灰盒
          image: grey-box.svg
          text: |
            灰盒测试需要关于您的系统的部分信息，例如有限的凭据或受限的访问权限。这种方法模拟了一个拥有部分访问权限的恶意用户对您的基础设施进行的攻击。它在重现现实场景的同时评估内部安全性。这是黑盒和白盒方法之间的折衷，提供了平衡的分析。

  - block: cta-card
    content: 
      title: 找到适合您需求的安全和渗透测试服务
      text: |
        随着网络安全漏洞继续威胁各行业公司的稳定性，找到合适的供应商来进行有效且有益的安全评估变得尤为重要。我们了解每个组织都有独特的安全目标和挑战，我们努力定制我们的服务以满足您的需求。
      button:
        text: 请求报价
        url: /zh/contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---
