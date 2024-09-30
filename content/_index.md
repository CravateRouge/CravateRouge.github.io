---
title: 'Home'
date: 2023-10-24
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: hero
    content:
      title: Tighten your security
      text: 🧱 Solutions tailored for your needs  🧱
      primary_action:
        text: Contact Us
        url: mailto:contact@cravaterouge.com
        icon: rocket-launch
      secondary_action:
        text: Discover our services
        url: /#services
      # announcement:
      #   text: "Announcing the release of version 1."
      #   link:
      #     text: "Read more"
      #     url: "/blog/"
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
          filename: bg-triangles.svg
          filters:
            brightness: 0.5
  - block: stats
    content:
      items:
        - statistic: "100+"
          description: |
            Missions executed
        - statistic: "6Yrs+"
          description: |
            Field experience
        - statistic: "1k5+"
          description: |
            GitHub stars  
            for security tools

    design:
      # Section background color (CSS class)
      css_class: "bg-gray-100 dark:bg-gray-900"
      # Reduce spacing
      spacing:
        padding: ["1rem", 0, "1rem", 0]
  - block: features
    id: services
    content:
      title: Services
      text: Build your security maturity 🧱
      items:
        - name: Protect
          icon: lock-closed
          description: Harden your systems by adding measures adapted to your infrastructure
        - name: Audit
          icon: bolt
          description: Test your security with pentests to see if it can resist to malicious actors! 
        - name: Investigate
          icon: magnifying-glass
          description: Conduct a post-attack investigation to understand what happened
  - block: cta-image-paragraph
    id: services-details
    content:
      items:
        - title: Protect your infrastructure
          text: Harden your systems by adding measures adapted to your infrastructure
          feature_icon: lock-closed
          features:
            - "Keep your data safe"
            - "Design solutions optimized for your business"
            - "Implement Security Standards"
          # Upload image to `assets/media/` and reference the filename here
          image: protect.jpg
          button:
            text: Ask Quotation
            url: mailto:contact@cravaterouge.com
        - title: Audit your assets
          text: Test your security with pentests to see if it can resist to malicious actors!
          feature_icon: bolt
          features:
            - "Identify weaknesses in your applications with pentests using automated tools and advanced attacks"
            - "Test your resilience against malicious actors with attack simulations (red team, insider compromission...)"
            - "Prices start at 1000$"
          # Upload image to `assets/media/` and reference the filename here
          image: audit.jpg
          button:
            text: Order Now
            url: mailto:contact@cravaterouge.com
        - title: Conduct post-attack investigation
          text: Conduct a post-attack investigation to understand what happened
          feature_icon: magnifying-glass
          features:
            - "Identify which path the malicious actor took to secure it"
            - "Evaluate the damages"
            - "Establishing an attacker profile"
            - "150$/hr"
          # Upload image to `assets/media/` and reference the filename here
          image: investigate.jpg
          button:
            text: Request Now
            url: mailto:contact@cravaterouge.com
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

  - block: logos
    content:
      title: "They trusted us"
      items:
        - logo: "axa.png"
        - logo: "bnp.png"
        - logo: "europe.png"
    design:
      # Reduce spacing
      spacing:
        padding: ["1rem", 0, "1rem", 0]

  - block: cta-card
    content:
      title: Strengthen your infrastructure
      text: As easy as 1, 2, 3!
      button:
        text: Contact Us
        url: contact@cravaterouge.com
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
