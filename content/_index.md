---
title: 'Home'
date: 2024-10-28
type: landing

design:
  # Default section spacing
  spacing: "1rem"

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
            Missions executed
        - statistic: "6Yrs+"
          description: |
            Field experience
        - statistic: "1500+"
          description: |
            Security experts
            <a class="underline hover:no-underline" href="https://github.com/CravateRouge/bloodyAD" target="_blank" rel="noopener noreferrer">apppreciating our work</a>
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

  - block: features
    id: services
    content:
      title: Our Services
      text: 🧱 Build your security maturity 🧱
      items:
        - name: Audit
          icon: bolt
          description: Test your security with pentests to see if it can resist to threat actors!
        - name: Secure Workspace
          icon: cpu-chip
          description: Our new solution to keep your digital workspace secured
        - name: Protect
          icon: lock-closed
          description: Harden your systems by adding measures adapted to your infrastructure
        - name: Training
          icon: academic-cap
          description: Cybersecurity will have no secrets for you!
        - name: Investigate
          icon: magnifying-glass
          description: Conduct a post-attack investigation to understand what happened
        - name: Softwares
          icon: beaker
          description: Get protection, detection and audit tools tailored to your needs
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph
    id: services-details
    content:
      items:
        - title: Secure Workspace
          text: For securing your infrastructure in a few steps
          feature_icon: cpu-chip
          features:
            - "Unified and strengthen access"
            - "Centralized management of your IT assets"
            - "Take back control of your Data (GDPR compliance)"
            - "Based on Microsoft™ technologies (no third-party required)"
          image: secureWorkspace.png
          button:
            text: More info
            url: articles/secure-workspace/

        - title: Protect your infrastructure
          text: Harden your systems by adding measures adapted to your infrastructure
          feature_icon: lock-closed
          features:
            - "Keep your data safe"
            - "Design solutions optimized for your business"
            - "Implement Security Standards"
          # Upload image to `assets/media/` and reference the filename here
          image: protect.png
          button:
            text: Ask Quotation
            url: contact/
        - title: Audit your assets
          text: Test your security with pentests to see if it can resist to threat actors!
          feature_icon: bolt
          features:
            - "Identify weaknesses in your applications with pentests using automated tools and advanced attacks depending of your needs"
            - "Test your resilience against threat actors with attack simulations (red team, insider compromission...)"
          # Upload image to `assets/media/` and reference the filename here
          image: audit.png
          button:
            text: Order Now
            url: contact/

        - title: Train your teams
          text: Cybersecurity will have no secrets for you!
          feature_icon: academic-cap
          features:
            - "Raise your teams' safety awareness with mail phishing campaigns"
            - "Learn to develop secured softwares"
            - "Trainings on security topics specific to your needs"
          # Upload image to `assets/media/` and reference the filename here
          image: training.png
          button:
            text: Book now
            url: contact/

        - title: Conduct post-attack investigation
          text: Conduct a post-attack investigation to understand what happened
          feature_icon: magnifying-glass
          features:
            - "Identify which path the threat actor took and secure it"
            - "Evaluate the damages"
            - "Establishing an attacker profile"
          # Upload image to `assets/media/` and reference the filename here
          image: investigate.png
          button:
            text: Request Now
            url: contact/
        
        - title: Custom softwares
          text: Get protection, detection and audit tools tailored to your needs
          feature_icon: beaker
          features:
            - "Protect your softwares of vulnerabilities with custom patching"
            - "Detect targeted threats"
            - "PoC exploits and malwares to simulate a threat"
          # Upload image to `assets/media/` and reference the filename here
          image: craft.png
          button:
            text: Ask Quotation
            url: contact/
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

  # - block: logos
  #   content:
  #     title: "They chose us"
  #     items:
  #       - logo: "axa.png"
  #       - logo: "bnp.png"
  #       - logo: "europe.png"
  #   design:
  #     # Reduce spacing
  #     spacing:
  #       padding: ["1rem", 0, "1rem", 0]

  - block: cta-card
    content:
      title: Strengthen your infrastructure
      text: Our team is taking care of it!
      button:
        text: Contact Us
        url: contact/
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
