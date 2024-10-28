---
title: 'Accueil'
date: 2024-10-02
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: hero
    content:
      title: Renouez avec la sécurité
      text: 🌎 Solutions disponibles à l'international 🌎
      primary_action:
        text: Contactez-nous
        url: contact/
        icon: rocket-launch
      secondary_action:
        text: Découvrez nos services
        url: /#services
      announcement:
        text: "Lire nos derniers"
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
  - block: stats
    content:
      items:
        - statistic: "100+"
          description: |
            Missions réalisées
        - statistic: "6Ans+"
          description: |
            D'expérience sur le terrain
        - statistic: "1k5+"
          description: |
            GitHub stars  
            pour nos outils de sécurité

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
      text: 🧱 Construisez une infrastructure robuste 🧱
      items:
        - name: Secure Workspace
          icon: cpu-chip
          description: Notre nouvelle solution pour un espace numérique d'entreprise sécurisé
        - name: Protection
          icon: lock-closed
          description: Renforcez vos systèmes en implémentant des mesures adaptées
        - name: Audit
          icon: bolt
          description: Testez la résistance de votre infrastructure face aux attaques!
        - name: Formation
          icon: academic-cap
          description: Devenez un As de la cybersécurité!
        - name: Investigation
          icon: magnifying-glass
          description: Menez une enquête post-attaque pour comprendre les tenants et les aboutissants
        - name: Logiciels
          icon: beaker
          description: Obtenez des outils de protection, détection et audit correspondants à vos besoins
  - block: cta-image-paragraph
    id: services-details
    content:
      items:
        - title: Secure Workspace
          text: Une infrastructure sécurisée en quelques clics
          feature_icon: cpu-chip
          features:
            - "Accès unifiés et renforcés"
            - "Gestion du parc informatique centralisée"
            - "Reprenez le contrôle de vos données (conformité RGPD)"
            - "Basé sur les technologies Microsoft™ (Aucun logiciel tiers à installer)"
          image: secureWorkspace.jpeg
          button:
            text: En savoir plus
            url: articles/secure-workspace/

        - title: Protégez votre infrastructure
          text: Renforcez vos systèmes en implémentant des mesures adaptées
          feature_icon: lock-closed
          features:
            - "Mettez vos données en sécurité"
            - "Solutions adaptées à votre entreprise"
            - "Implémentez les dernières normes de sécurité"
          # Upload image to `assets/media/` and reference the filename here
          image: protect.jpg
          button:
            text: Demandez un devis
            url: contact/

        - title: Auditez vos systèmes
          text: Testez la résistance de votre infrastructure face aux attaques!
          feature_icon: bolt
          features:
            - "Identifiez les faiblesses de vos applications via des pentests en utilisant des outils automatisés et/ou des attaques avancées en fonction de vos besoins"
            - "Vérifiez votre robustesse face aux cybercriminels en réalisant des simulations d'attaques (red team, menace interne...)"
            - "A partir de 906€ HT"
          # Upload image to `assets/media/` and reference the filename here
          image: audit.jpg
          button:
            text: Commandez un audit
            url: contact/

        - title: Formez vos équipes
          text: Devenez un As de la cybersécurité!
          feature_icon: academic-cap
          features:
            - "Sensibilisez vos équipes aux menaces avec des campagnes de phishing mail"
            - "Apprenez à développer de manière sécurisée"
            - "Formations sur des sujet cyber spécifiques à vos besoins"
            - "Formations à partir de 2800€ HT"
          # Upload image to `assets/media/` and reference the filename here
          image: training.jpg
          button:
            text: Réservez maintenant
            url: contact/
        
        - title: Menez une investigation post-attaque
          text: Menez une enquête post-attaque pour comprendre les tenants et les aboutissants
          feature_icon: magnifying-glass
          features:
            - "Identifiez les chemins empruntés par les cybercriminels et sécurisez-les"
            - "Évaluez l'impact"
            - "Profilez l'attaquant"
            - "136€ HT l'heure"
          # Upload image to `assets/media/` and reference the filename here
          image: investigate.jpg
          button:
            text: Contactez-nous dès maintenant
            url: contact/
          
        - title: Logiciels sur mesure
          text: Obtenez des outils de protection, détection et audit correspondants à vos besoins
          feature_icon: beaker
          features:
            - "Protégez vos logiciels des vulnérabilités avec des patchs personnalisés"
            - "Détectez les menaces ciblées"
            - "Simulez des attaques avec des preuves de concept d'exploits et de malware personnalisés"
          # Upload image to `assets/media/` and reference the filename here
          image: craft.jpg
          button:
            text: Demandez un devis
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

  - block: logos
    content:
      title: "Ils nous ont fait confiance"
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
      title: Renforcez votre infrastructure
      text: Notre équipe s'en occupe!
      button:
        text: Contactez-nous
        url: contact/
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
