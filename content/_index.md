---
title: 'Home'
type: landing

design:
  # Default section spacing
  spacing: "0rem"

sections:
  - block: hero
    content:
      title: Keep it tight with your security
      text: 🌎 Solutions available worldwide 🌎
      primary_action:
        text: Contact Us
        url: contact/
        icon: rocket-launch
      secondary_action:
        text: Discover our services
        url: /#services
      announcement:
        text: "Check our latest"
        link:
          text: "articles"
          url: "/articles/"
    design:
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
            Missions executed
        - statistic: "6Yrs+"
          description: |
            Field experience
        - statistic: "1500+"
          description: |
            Security experts [apppreciating our work](https://github.com/CravateRouge/bloodyAD)
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: collection
    content:
      title: They talk about us
      text: See what the Industry is saying about us
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
      title: Our Services
      text: 🧱 Build your security maturity 🧱
      items:
        - name: Audit
          icon: bolt
          description: |
            [Test your security with pentests to see if it can resist to threat actors!](#auditsum)
        - name: Protect
          icon: lock-closed
          description: |
            [Harden your systems by adding measures adapted to your infrastructure](#protectsum)
        - name: Training
          icon: academic-cap
          description: |
            [Cybersecurity will have no secrets for you!](#trainingsum)
        - name: Phishing simulation
          icon: envelope
          description: |
            [Train your employees to identify cyber threats by implementing powerful phishing simulations](#phishingsum)
        - name: Microsoft 365 Integration
          icon: cpu-chip
          description: |
            [Integrate M365 and Azure Cloud to your business to increase productivity and security](#m365sum)
        - name: Tailored services
          icon: beaker
          description: |
            [If you have a specific need let us know](#tailoredsum)
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph-custom
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      items:
        - title: Audit your assets
          text: Test your security with pentests to see if it can resist to threat actors!
          feature_icon: bolt
          features:
            - "Identify weaknesses in your applications with pentests using automated tools and advanced attacks depending of your needs"
            - "Test your resilience against threat actors with attack simulations (red team, insider compromission...)"
          image: audit.png
          button:
            text: Order Now
            url: contact/
          id: auditsum
        - title: Protect your infrastructure
          text: Harden your systems by adding measures adapted to your infrastructure
          feature_icon: lock-closed
          features:
            - "Keep your data safe"
            - "Design solutions optimized for your business"
            - "Implement Security Standards"
          image: protect.png
          button:
            text: Ask Quotation
            url: contact/
          id: protectsum
        - title: Train your teams
          text: Cybersecurity will have no secrets for you
          feature_icon: academic-cap
          features:
            - "Learn to develop secured softwares"
            - "Trainings on security topics specific to your needs"
          image: training.png
          button:
            text: Book now
            url: contact/
          id: trainingsum
        - title: Simulate a phishing attempt
          text: Raise your teams’ safety awareness
          feature_icon: envelope
          features:
            - "Mirrors real-world cyber threats"
            - "Educate end users on common phishing tactics"
            - "Customizable phishing scenarios"
          image: phishing.png
          button:
            text: Learn more
            url: contact/
          id: phishingsum
        - title: Microsoft 365 & Azure Integration
          text: All-in-one to increase your productivity and security
          feature_icon: cpu-chip
          features:
            - "Unified and strengthen access"
            - "Centralized management of your IT assets"
            - "Take back control of your Data (GDPR compliance)"
            - "Based on Microsoft™ technologies (no third-party required)"
          image: secureWorkspace.png
          button:
            text: More info
            url: services/integration/
          id: m365sum
        - title: Tailored services
          text: A specific cybersecurity need?
          feature_icon: beaker
          image: craft.png
          button:
            text: Tell us your project
            url: contact/
          id: tailoredsum

  - block: cta-card
    content:
      title: Strengthen your infrastructure
      text: Our team is taking care of it!
      button:
        text: Contact Us
        url: contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---
