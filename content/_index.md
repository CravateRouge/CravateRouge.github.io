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
        about: Who are we?
      text: |
        I am Baptiste Crépin, CEO and security expert at CravateRouge Ltd.

        I began my journey into cybersecurity with a Master's degree from [Paris-Saclay University](https://www.shanghairanking.com/institution/paris-saclay-university). Following this, I gained extensive experience as a red team operator at [AXA Insurance](https://www.axa.com/en/about-us/axa-worldwide) specializing in Microsoft environments. This role gave me the opportunity to simulate real-world attacks to identify and mitigate vulnerabilities.

        In addition, I share my research and develop advanced tools helping security experts to provide you cutting-edge protection. In particular, I had the honor of presenting one of my tool at the [Black Hat of Las Vegas](https://www.blackhat.com/us-22/arsenal/schedule/#bloodyad-26883), one of the largest computer security events in the world.

        That's why I decided to found CravateRouge Ltd, to bring my expertise directly to you. With personalized, comprehensive cybersecurity services specialized in Microsoft products, we ensure your digital assets are protected by the best in the field. Trust in our expertise to safeguard your future.
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
            Security experts <a href='https://github.com/CravateRouge/bloodyAD'>apppreciating our work</a>
    spacing:
      padding: ["1rem", 0, "1rem", 0]
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
            <a href='#auditsum'>Test your security with pentests to see if it can resist to threat actors!</a>
        - name: Protect
          icon: lock-closed
          description: |
            <a href='#protectsum'>Harden your systems by adding measures adapted to your infrastructure</a>
        - name: Training
          icon: academic-cap
          description: |
            <a href='#trainingsum'>Cybersecurity will have no secrets for you!</a>
        - name: Phishing simulation
          icon: envelope
          description: |
            <a href='#phishingsum'>Train your employees to identify cyber threats by implementing powerful phishing simulations</a>
        - name: Microsoft 365 Integration
          icon: cpu-chip
          description: |
            <a href='#m365sum'>Integrate M365 and Azure Cloud to your business to increase productivity and security</a>
        - name: Tailored services
          icon: beaker
          description: |
            <a href='#tailoredsum'>If you have a specific need let us know</a>
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
      spacing:
        padding: [0, 0, 0, 0]

  - block: cta-image-paragraph
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
            text: Choose your audit type
            url: services/audit/
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
            url: services/phishing
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
      spacing:
        padding: ["1rem", 0, 0, 0]
---
